# vue-strcalc

> A String calculator written in Vue.js

## Features!
- Input is of the form `//[delimiter]\n[delimiter separated numbers]`.
- Example: `//;\n1;3;4` has an output of 8.
- Other examples: `//$\n1$2$3` outputs 6, and `//@\n2@3@8` outputs 13.
- Supports multiple arbitrary-length delimiters: `//@,$\n1@4$5` outputs 10 and `//@@,###\n2@@4###2` outputs 8.
- Numbers greater than 1000 are ignored: `//;\n1;2;1001` outputs 3.
- Putting in a negative number throws an exception and aborts the entire system: “Negatives not allowed”.
- The program assumes correct inputs for simplicity.

### Tech
vue-strcalc uses Vue.js as a JavaScript framework for easier DOM referencing and for implementing reactivity in the program.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev
```

### A note on testing
Due to time constraints, test cases have not yet been created to verify the input. Future iterations of this program will implement testing the SFC using Jest.