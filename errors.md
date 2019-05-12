# Errors

The policy for the DlangGraphicsWG about errors is the following:

1. Internal functions may use any mechanism, left at the discretion of the implementer

2. In the public parts of the library that have to follow `-betterC` (TBD):

  - If the function is publicly exported by a module **and has to report an error message**,
    use `Result!T` a combination of a `T` an a message `string`.
    If the message `string` is null then the `T` is valid.

  - Else if the function doesn't have to report an error message, use error codes (`bool` return and `out` params).


3. In the parts of the library that don't have to follow `-betterC` (TBD)

Use normal D exceptions.
