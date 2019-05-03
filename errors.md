# Errors

The policy for the DlangGraphicsWG about errors is the following:

## In the parts of the library that have to follow `-betterC`:

- If the function is publicly exported by a module **and has to report an error message**,
  use `Result!T` a combination of a nullable `T` an a message `string`.

- Else if the function is part of the public API and doesn't have to report an error message, 
  use error codes.

- Else If the function is private to a module, use either error codes or `Result!T`.


## In the parts of the library that don't have to follow `-betterC` (TBD)

Use normal D exceptions.
