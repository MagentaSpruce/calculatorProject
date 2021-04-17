# calculatorProject
Simple old-school style calculator made following The Odin Project curriculum

This is designed after an old school desktop calculator.

This project helped me to practice and better understand:
1) HTML/CSS 
2) Creating and implementing functions
3) DOM manipulation
4) Testing
5) Javascript coding
6) Developing logic

This project has the following known bugs which need correcting:
1) When using the ON key - the 0 needs to be made to disappear when the 1st user input is made and that user input needs to be recorded as the 1st argument.
2) The second argument is unable to show repeat values making it hard for a user to keep accurate track of their second argument inputs.
3) Sometimes, answers can be output which throw integers/decimals to overflow the output screen.
4) When using a decimal inside of both arguments, the output returns NaN

A general walkthrough of the project and it's noteworhy code is as follows:

To begin, the DOM elements and starting parameters must be constructed.
```JavaScript
const output = document.querySelector('#output');
const digits = document.querySelectorAll('button');


let operator = '';
let firstArg = '';
let secondArg = '';

```

Following this the functions for each of the main operations (+ , / , x , -, %) needed to be created - remembering to account for dividing against 0.
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

At this point the arguments have been created and are set to empty and the initial functions have been made. Next a function is created to take the previously made functions and use them to calculate the two inputs according to the operation chosen.
This is where new methods could be added, such as squareroot or exponents.
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

At this stage it is time to set up the parameters for the two arguments which will be used in calculations. Starting from the top, the first if() statement prevents double decimals. The second if() prevents adding nothing. The third if() splits the firstArg into an array, deletes the last unit added, then rejoins the array back to a string and outputs the string. The fourth if() statement prevents taking a % of nothing and the last if() block is what converts the user input into a percentage point and as well prepares it to operate with the incoming second argument.
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

The second argument follows the first but with less parameter checks.
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

Now that the arguments, operators, and operator functions have been set-up, now it is time to get the buttons ready for user inputs. Starting from the top again the forEach loops through each button adding a click event listener to each one. The first if() statement is to ensure a rest occurs if an operation was previously finished. If a previously calculation has been finished, this code will reset the calculator so new input can be input and operated on. The rest of the code block reads like this: If the button pushed is a number or a . or a &larr; or a % AND nothing has been input yet then that input will become the first argument. However, if there already is a first argument which is not 0 then that input will become the 2nd argument. Else if the button is not a number and not the = key AND the operator equals 0 then the button input (like the x or % button) will become the value loaded - but this value won't show on the screen.
```JavaScript
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

With everything now set up and in working order the time has come to perform the operations. The first if() block is there so that if the calculator is in a continuing calculation the output will be the evaluation of the previously made arguments. This makes it easy for the user to keep track of their current sum. The last code block dealing with firstArg is there to reset the secondArg and the operator so that continuing calculations can be input and summed.
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

Finally the last task is to display the results of the operations to the user display. The first if() statement reoututs the 1st argument if nothing is entered for the 2nd. The last lines set the previous answer as the first argument so additional calculations can be made upon the previous answer. The output.textcontent code line is what runs the operation and displays the result.
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

***End walkthrough - this is not all of the code. Some code has been left out as it was not considered noteworthy. Please check the actual code files for the full content***
