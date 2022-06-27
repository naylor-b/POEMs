POEM ID: 068  
Title: Modify the behavior of set_input_defaults.  
authors: @naylor-b  
Competing POEMs:  
Related POEMs:  
Associated implementation PR: (not submitted yet)

Status:

- [x] Active
- [ ] Requesting decision
- [ ] Accepted
- [ ] Rejected
- [ ] Integrated


## Motivation

The `set_input_defaults` method of Group was originally intended to disambiguate value, shape,
and/or units used to create an `_auto_ivc` output when that output was connected to more
than one input, but there are cases where the connected output does not belong to `_auto_ivc`. This
can happen if the `set_input_defaults` was added during development of a Group that is intended
to be reused in multiple models.  In some of those models the inputs in question might be 
connected to `_auto_ivc` outputs while in others they may not.  The Group developer doesn't know
how the Group might be connected in the future.

In cases where the inputs are *not* connected to an `_auto_ivc` output, the current behavior of
`set_input_defaults` is that it is ignored.  In cases where execution order of the connected
components matches the data flow order, this is intuitive, because regardless of the values contained
in the connected inputs, those values will be overwritten by the value contained in the connected
output prior to execution of the component that contains the input.  But in cases where data flow
order doesn't match the execution order, ignoring the input value specified in `set_input_defaults` 
is somewhat less intuitive.  A user might think that because the component that owns the
connected output does not execute prior to the component owning the input, that whatever
value was specified in `set_input_defaults` would be used as the input for the first execution of
of the component that owns the input.  This would match the behavior of calling `set_val` on 
the Problem and specifying the input value.


#TODO: just realized that calling set_val on an output will *always* override connected input
values, even if the output component doesn't run until after the input comp.

#TODO: Also, if an input is NOT connected to an auto_ivc, why should a user expect their initial
input value to not be overridden?  That would only possibly be the case for UBC execution, so do
we even really want to deal with that, or should we just explain the underlying simple rule, which
is that prior to component execution (NL - always fwd), the connected output value is ALWAYS
copied to the input, EVEN if the input component is executing BEFORE the output component.

#TODO:  So in general, a call to `set_input_defaults` will have no effect, save possibly generating
an error if you mess up the shapes/units/value, unless the connected output is an auto_ivc.
If it IS connected to an auto_ivc, any value set in the `set_input_defaults` call will end up
being the initial value of the input when the input component executes, but only because the
`set_input_defaults` call set the output value in the auto_ivc that then gets copied into the
component input just prior to the input component's execution.


## Proposed solution



## Example



