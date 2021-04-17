# calculatorProject
Simple old-school style calculator made following The Odin Project curriculum

This project follows the design of an old simple desktop calculator.

This project better helped me to understand and work with:
1) Web design
2) HTML/CSS
3) Designing and implementing functions
4) Designing and implementing game logic
5) Reading/writing/understand JavaScript code
6) DOM manipulation

***The following bugs/improvements need to be fixed:
*** 1) When using the ON key: To make the 0 disappear when the 1st button is pressed and to log what that initial button is as the first argument.
*** 2) Make multiple digits appear when using the 2nd argument
*** 3) Limit the possible number of decimal places in an answer to 8.
*** 4) Work out the bug involving 1st and 2nd arguments decimal usage (currently non-functional).

A walk through of the JavaScript code with guided explanation is below:

First, the project needed to have its DOM elements specified and its initial parameters set.

```JavaScript
const output = document.querySelector('#output');
const digits = document.querySelectorAll('button');


let operator = '';
let firstArg = '';
let secondArg = '';
```

Next the functions for each of the main operators (+, -, x, /) needed to be created. 

```JavaScript

const add = function(a,b){
    return parseFloat(a) + parseFloat(b);
}

const subtract = function(a,b){
    return parseFloat(a) - parseFloat(b);
}

const multiply = function(a,b){
    return parseFloat(a) * parseFloat(b);
}

const divide = function(a,b){
    if(parseFloat(b) === 0){
        return `0 can't be divided`;
    } else{
        return parseFloat(a) / parseFloat(b)
    }
}
```
Following this, the functions and arguments which have been created now need the ability to be operated on - a switch statement was used to accomplish this. The switch statement will run a certain operation depending upon the case selected. This is where new methods could be added in the future, such as square root or power.

```JavaScript
const operate = function(operator, num1, num2){
    switch(operator){
        case '+':
            return add(num1, num2).toFixed(1,8);
            break;
        case '-':
            return subtract(num1, num2).toFixed(1,8);
            break;
        case 'x':
            return multiply(num1, num2).toFixed(1,8);
            break;
        case '/':
            return divide(num1, num2).toFixed(1,8);
            break;
        case '%':
            return ((num1/num2)*100).toFixed(1,8);
            break;
    }
}
```
Now that the arguments, functions and the manner in which to combine them have been created it is time to set up the parameters of the first argument(a/num1). The first if() statements prevents double decimals. The second if() prevents the addition of nothing. The third if() splits the first argument into an array, removes the last object, rejoins the array and outputs the result. The fourth if() prevents taking a % of nothing. The fifth and last large if() block converts the user input to a percent and prepares it for accepting another argument in which to operate on.

```JavaScript
const firstArgOps = function(input){
    
    
    if(input === '.' && firstArg.includes('.')){
        return 'lollipop';
    }
    if(input === '+' && output.textContent === ''){
        return;
    }
    if(input === '&larr;'){
        firstArg = firstArg.split('');
        firstArg.pop();
        firstArg = firstArg.join('');
        output.textContent = firstArg;
        return;
    }
    if(input === '%' && output.textContent === ''){
        return;
    }
    if(input === '%'){
        firstArg = (parseFloat(firstArg)/100);
        firstArg = firstArg.toString();
        output.textContent=firstArg;
        return;
    }
    firstArg += input;
    output.textContent += input;
    boundsCheck();
}
```

The second argument(b/num2) follows:

```JavaScript
const secondArgOps = function(input){
    
    if(secondArg === '' && output.textContent !== ''){
        output.textContent = '';
    }
    if(input === '.' && secondArg.includes('.')){
        return;
    }
    if(input === '%'){
        secondArg = (parseFloat(secondArg)/100);
        secondArg = secondArg.toString();
        output.textContent = secondArg;
        return;
    }
    secondArg += input;
    output.textContent =+ input;
}
```

This function does the same as the first with less checks. One note is the first if() statement which is there to re-output the 1st argument if nothing is entered for the 2nd argument.

After all of this has been done we are ready to set the behavior of our buttons.

Starting from the top and going down: The first if() statement is there to ensure a reset occurs if an operation was already finished. This checks for that occurence and it it is true then it resets the calculator so new input can be taken and operated on. Following that, the rest of the code block basically reads like: If the button pushed is a #, ., % or &larr; and nothing has been input yet then that input will become the first argument. If there already is a first argument that is not 0 however then that input will become the 2nd argument. Else if the button isnt a # and not the = key AND the operator equal to 0 then the button input (like the % or the / button) will become the value loaded - however this value will not appear on the screen.

```javaScript
digits.forEach(digit => {
    
    digit.addEventListener('click', function(){
        if(output.textContent.includes("Answer:") && Number.isInteger(parseFloat(digit.textContent))){
            clearAll();
        }
        
        if(Number.isInteger(parseFloat(digit.textContent)) || digit.textContent === '.' || digit.textContent === '&larr;' || digit.textContent === '%'){
            if(operator === '')
            {
                firstArgOps(digit.textContent);
            }
            else if(firstArg !== '' && operator !== ''){
                secondArgOps(digit.textContent)
            }
        }
        else if(!Number.isInteger(parseFloat(digit.textContent)) && digit.textContent !== '=' && operator === ""){
            operator= digit.textContent;
            console.log(operator);
            output.textContent = '';
        }
    });
});
```

This is the portion of code which ties it all together so to speak - Starting from the if() statement, if a continued operation is made then the output will be the evaluation of the previously made arguments. This is so the user can keep score of there current sum. Following this, the last code block: firstArg = operate... resets the arguments so that the continuing evaluation can continue accepting new arguments.

```JavaScript
const operations = document.querySelectorAll('.operation');
operations.forEach(operation =>{
        operation.addEventListener('click', function(){
        console.log('yep');
        if (firstArg !== '' && secondArg !== ''){
            
            output.textContent = (`${operate(operator, parseFloat(firstArg), parseFloat(secondArg))}`);
            console.log('yes man')
            firstArg = operate(operator, parseFloat(firstArg), parseFloat(secondArg));
            secondArg = '';
            operator = operation.textContent;

        }
    });
});
```

When operations are finished (the calculation is done) and so everything needs to be wrapped up and output to the display for the user. Going from the top down once again, the first if() statement is re-outputting the 1st argument if nothing has been entered for the 2nd. The last else{} block are what allow for repeated calculations to be made (5 + 5 + 5 + 5...) and allows for repeated callculations to continued being made; ex: adding up a book ledgar.

```JavaScript
const enter = document.querySelector('#enter');
enter.addEventListener('click', function(){
    console.log('bop');
    if(secondArg === ''){
        output.textContent = firstArg;
    }
    else{
        console.log(`first:${firstArg} second:${secondArg} ${operator}`);
        output.textContent = (`${operate(operator, parseFloat(firstArg), parseFloat(secondArg))}`);
        
        firstArg = operate(operator, parseFloat(firstArg), parseFloat(secondArg));
        secondArg = '';
        operator='';
    }
    boundsCheck();

    
});
```

The other portion of codes are not really worthy of any note and so have been excluded. 
