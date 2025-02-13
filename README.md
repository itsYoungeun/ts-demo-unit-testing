## TypeScript Unit Testing: The Basics

Installing the TypeScript Compiler
Your browser can’t run TypeScript code. That means you need an additional step to convert TypeScript code to regular JavaScript so browsers can run it. You’ll need the TypeScript compiler to do that, so let’s install it. Run the following command from your terminal:
```
npm install -g typescript
```
The -g means you’ve installed it globally. Verify the installation using the following command:
```
tsc -v
```
You should see the current version displayed.

### Creating the Basic Folder Structure for the Project
Start by creating a folder for the project and accessing it:
```
mkdir tsdemo
cd tsdemo
```
In this folder, let’s create a src folder for the source code and a test folder for the unit tests:
```
mkdir src
mkdir tests
```
Now, let’s use npm to initiate a new project. On the root of the tsdemo folder, run:
```
npm init -y
```
When you use npm init, you’re usually asked several questions. The -y flag ensures we’re answering the defaults for all of these questions.

### Adding TypeScript to the Project
The next step is adding TypeScript as a development dependency to the project:
```
npm install typescript --save-dev
```
Your package.json file should now list TypeScript as a dependency.

Using your code editor of choice, create a file called index.ts inside the src folder. Paste the following code in it:
```
function add(numbers: string): number {
    let integers = numbers.split(',').map(x => parseInt(x));
    let negatives = integers.filter(x => x < 0);

    if (negatives.length > 0)
        throw new RangeError('Negatives are not allowed: ' + negatives.join(', '));

    return integers
        .filter(x => x <= 1000)
        .reduce((a, b) => a + b, 0);
}

let result = add('1, 2, 4, 5');
console.log(result);
```
As you can see, by the end of the file, we call the add method and print the result to the log. What about running the code? First, compile the file to regular JavaScript:
```
tsc index.ts
```
Now you can use Node.js to execute the resulting file:
```
node index.js
```
You should see “6” displayed on your terminal if everything worked right.

### Installing the Testing Tools
There are quite a few unit testing frameworks for JavaScript. We have to pick one, so let’s go with Jest.

Start by adding Jest as a development dependency. In the root of the project folder, execute the following:
```
npm install jest --save-dev
```
For the next step, you’ll have to install the ts-jest package to bridge the path between TypeScript and Jest, so to speak:
```
npm install ts-jest --save-dev
```
Finally, you must install the type definitions for Jest:
```
npm install @types/jest --save-dev
```

### Writing and Running Your First Test
Now you have the pieces in their places, so let’s start testing.

Using your code editor, create a file inside tests named index.test.ts. The file should have this code:
```
import { add } from '../src/index';

describe('testing index file', () => {
  test('empty string should result in zero', () => {
    expect(add('')).toBe(0);
  });
});
```
The file starts by importing the add function from its file. Then, it starts a new test suite—with the describe function—and, inside that, it creates the first test. The first test expresses that when we call add passing an empty string, we expect the result to be zero.

Are we ready to test? Not yet. Our importing doesn’t work. Since I’m using Visual Studio Code, I see a red line and a helpful error message.

What’s happening is that, since we don’t export the index.ts file as a module, it can’t be imported. This is easy to fix. Return to the index.ts file and add the word “export” before “function.” While you’re at it, remove the last two lines—you’ll no longer need them since from now on you’ll test the function using Jest.

Finally, create an additional file called “jest.config.js” at the root of the project. It should have the following content:
```
module.exports = {
  transform: {'^.+\\.ts?$': 'ts-jest'},
  testEnvironment: 'node',
  testRegex: '/tests/.*\\.(test|spec)?\\.(ts|tsx)$',
  moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node']
};
```
This file is necessary to perform the necessary transformation in order to turn TypeScript into regular JavaScript code.

Now you can run the test using the jest command.
```
npx jest
```
