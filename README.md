# BotBoiler
*Boilerplate code for Typescript based bots*

BotBoiler is base code to get you started with an enterprise scale Node+Typescript based bot. 

It's core tenets are that it must be composable, testable, extensible, adhere to separation of concerns and above all be simple, elegant and maintainable.

Create dialogs, use LUIS or QnA maker, call databases and other services, add authentication and a range of other bot functionality in a nice loosely coupled composable way. 

Inject functionality in to your dialogs as required to reduce complexity and increase testability. 

Unit test using the [Ava](https://github.com/avajs/ava) framework and stub and spy the Bot Framework with [Sinon](http://sinonjs.org/). 

**Note:** This is a work in progress, so much more to come. Please create issues / PRs if you find problems or can think of improvements. 

Thanks to [@geektrainer](https://github.com/GeekTrainer?tab=repositories) and others in the [Bot Builder Generator](https://github.com/MicrosoftDX/generator-botbuilder/) for the inspiration. 

## Prerequisites

Before you begin, it's recommended that you're across the following:

- [Bot Builder and the Bot Framework](https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-quickstart)
- Get an idea of how bot conversations work with [dialogs](https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-dialog-manage-conversation-flow)
- Read up on using Typescript in Visual Studio Code: [Visual Studio code and Typescript](https://code.visualstudio.com/docs/languages/typescript)
- [the Ava Javascript Unit Testing Framework](https://github.com/avajs/ava)
- Learn about stubs and spies using [Sinon](http://sinonjs.org/)
- Dependency Injection with [Inversify](http://inversify.io/)



## Getting Started

*Note* We are working on a [generator](https://github.com/MSFTAuDX/generator-botboiler) to help get started quickly which will be available soon. 

Clone [this](https://github.com/MSFTAuDX/BotBoiler) GitHub repo and launch VS Code from the base directory of the project. 

### Environment Settings

During development, you can place your settings in the ```.env``` file located in the root of the project. There are some placholders there already, and you can add more as you please. You'll have to edit ```_prepConfig()``` in ```startup.cs``` to add the mappings for any new settings. 

```typescript
 private _prepConfig(): IConfig {

    var sh = new serverHelper();

    this._config = {
        port: process.env.port || process.env.PORT || 3978,
        microsoftAppId: process.env.MICROSOFT_APP_ID,
        microsoftAppPassword: process.env.MICROSOFT_APP_PASSWORD,
        luisModelUrl: process.env.LUIS_MODEL_URL,
        serverType: sh.getServerType(),
        KBID: process.env.KBID,
        subscription: process.env.SUBSCRIPTION_KEY
    }

    return this._config;
}

```

### Set up a new bot 

You'll need to set up a new bot on the [Microsoft Bot Framework developer](https://dev.botframework.com/) site. You can get started with some basic code, but before long you'll need to register for a bot and get your AppId and Password.  Creating this is outside the scope of this document. 

Once created, pop your AppId and Password in both the ```.env``` file and use them to connect in the emulator. 

### Debugging your bot using the Bot Framework Emulator

Using the various methods below you can connect to your bot for local debugging. Using one of F5 Debugging in code or "Normal Dev Iteration" using ```tsc``` watch and ```nodemon``` you can connect to your bot using the [Bot Framework Emulator](https://docs.microsoft.com/en-us/bot-framework/debug-bots-emulator) on the default port of ```3978```.

For more info on the emulator [here](https://docs.microsoft.com/en-us/bot-framework/debug-bots-emulator) and downloads for Windows, Mac and Linux [here](https://github.com/Microsoft/BotFramework-Emulator/releases).

## Working with the framework

This project and documentation is based on [Visual Studio Code](https://code.visualstudio.com/) but you can of course use any tool that works well with Node and Typescript!

There are a few modes of development you might like to choose depending on your development phase. 

### Normal Dev Iteration

*Watch mode. You're working away and you want the code to recompile and the test bot server always be up to date.*

This mode is simple to start (instructions are for [Visual Studio Code](https://code.visualstudio.com/))

- Kick off a build using ```crtl-shift-b```. On the output screen you should see something like ```10:14:11 AM - Compilation complete. Watching for file changes.```
- Open a new terminal window (```ctrl-~``` is the default shortcut to bring this up.)
- Type ```npm run outputwatch``` to watch output file changes using nodemon. 
- Edit your files, connect to your bot and off you go!

By using ```tsc``` in watch mode your Typescript code will be transpiled to the output/run directory. This will then cause ```nodemon``` to restart your server with the latest changes every time you save. Nice!  You'll see outputs to this effect in your terminal window. 

<img src="https://user-images.githubusercontent.com/5225782/27891601-0cfaae16-623e-11e7-94c8-cf656789258e.gif" width="640" alt="nodemon"/>

**Note on working with many terminal windows in VS Code** When working with multiple terminals, it can become cumbersome to need to switch between them with the mouse all the time. Luckily you can add a keyboard short cuts. Press ```ctrl-shift-p```, type ```keyboard``` and select ```Preferences: Open Keyboard Shortcuts File```. Add the following shortcuts:

```json
// Place your key bindings in this file to overwrite the defaults
[
    {
        "key": "ctrl-alt-left",
        "command": "workbench.action.terminal.focusPrevious"
    },
    {
        "key": "ctrl-alt-right",
        "command": "workbench.action.terminal.focusNext"
    }
]
```

**Note** Pressing ```ctrl-shift-b``` in VS Code will kick off a build that will never stop - i.e. runs ```tsc``` in watch mode which will never exit until you break out. You'll know this is the case because of the "spinny thing" &trade; in the bottom left hand corner. To kill it, just run the build again - a message at the top of the screen will ask if you want to terminate it. 

![spinnything](https://user-images.githubusercontent.com/5225782/27891293-5c4e6554-623c-11e7-8834-9921633ddbb8.gif)

### Debug Dev

*Debug mode. You've been working away, and have a bug that you need to debug.* 

**Note** Make sure other build / monitor processes are not running by switching to the terminal tabs and pressing ```ctrl-c``` e.g. you've run ```tsc``` and ```nodemon``` using the steps outlined above. See the note above about the spinny thing and breaking out of a ```tsc``` watch. Also make sure that test monitors are not running as this might cause unknown stuff to occur. 

Just press ```F5``` and the app should build, and a debugger will attach ready for you to debug. Thanks to [source maps](https://code.visualstudio.com/docs/languages/typescript#_javascript-source-map-support) you create all your breakpoints in your TS files and VS Code will manage breaking the right spot (even though it's running the transpiled .js files)

<img src="https://user-images.githubusercontent.com/5225782/27891765-09a6a570-623f-11e7-9c5d-daab48561598.PNG" width="720" alt="debugging in code"/>

### Normal Test

*You're working on unit/integration tests. You want them to just run as you work.*

**Note** See the section on unit testing for more info here, this is just the getting started bit.

This is the fun part. It's a bit like the "Normal Dev Iteration" above where it runs ```tsc``` in watch mode, but this time instead of ```nodemon``` firing up a server, we use ```ava``` in watch mode to automatically run your tests - when ever you save. 

Make sure the build isn't running (yep - spinny thing no no). 

You'll need two terminals for this. In the first one, type ```npm run testwatchbuild```. This will kick off ```tsc``` in watch mode for the tests sub folder which outputs to output/test/tests. It will also publish the other src folder under output/test because it is a dependency.

The next thing is to start ```ava``` in watch mode. Type ```npm run testwatchava```. Ava will now wait for changes on the transpiled files and will automatically run your tests. 

<img width="320" src="https://user-images.githubusercontent.com/5225782/27892089-00f38b1c-6241-11e7-8b6f-fa5b034fdeb7.gif"/>

### Debug Test

*Unit/integration tests are not behaving. You need some help debugging them*

You can connect a debugger for your tests. The way ava works, you cannot simply set your startup file as usual and attach the code debugger. We have however set up a launch task that does the special ava way. 

First, you need to select which test file to attach to. Open ```launch.json``` and find the task named "Launch Test". Edit the args to point to the **transpiled** file location. Don't worry, you can still set breakpoints in your original TS files - VS Code handles all that for you again thanks to [source maps](https://code.visualstudio.com/docs/languages/typescript#_javascript-source-map-support). 

```json
"args": [
    "${workspaceRoot}/output/test/tests/run/samples/dialog/testLuisDialog.js"
],
```

From the debug menu ![bot_debugbutton](https://user-images.githubusercontent.com/5225782/27892175-6e35a9a8-6241-11e7-82e1-050fc99f7bf8.PNG)
 in VS Code select "Launch Test" from the drop down and press ```F5```. 

 **Note** Sometimes the break points can get a bit out of wack - especially with async code. If this is the case, then use ```debugger;``` in your code to do it manually - that seems to work better!


## Create a dialog

Dialogs are the center of bots. Adding them is super simple in this framework. Dialogs are exposed via the ```dialogIndex``` module. Any dialog that is exposed here will be automatically added to the IOC Container ([Inversify](http://inversify.io/)) and added to the bot dialog. Any dialog exposed in this way can ask for any other component or service that has been registered to be injected at runtime. 

### Add the dialog

**Note** there are some [samples](https://github.com/MSFTAuDX/BotBoiler/tree/master/src/dialogs/samples) you can follow. 

Let's create a super simple two step dialog. 

Dialogs are javascript classes in this framework. They implement the ```IDialog``` interface. This interfaces says we need to expose each step as an array via the ```waterfall()``` getter. 

At the end of the day Dialogs are still based on the [waterfall](https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-dialog-manage-conversation-flow) approach if you'd like to do further reading on that.  

First, create a new dialog under src/dialogs. Use this template to get started. It sets up the dialog for injection etc. In this example we'll not take any injected dependencies for simplicity (more on that further down). 

```typescript
import { serviceBase } from './../system/services/serviceBase';
import * as builder from 'botbuilder';
import { injectable, inject } from "inversify";

import * as contracts  from '../system/contract/contracts';

@injectable()
export default class someDialog extends serviceBase implements contracts.IDialog{
    public id:string = 'someDialog';
    public name:string ='someDialog';
    public trigger:RegExp = /^somedialog$/i

    public get waterfall(): builder.IDialogWaterfallStep[]{
        return [this.step1.bind(this), this.step2.bind(this)];
    }

    step1(session: builder.Session, args:any, next:Function)  {  

    }

    step2(session: builder.Session, results:builder.IDialogResult<string>, next:Function) { 

    }
}
```

#### Some notes
 This bot has a ```RegExp``` trigger - in this case it will be triggered if you use types "somedialog". You can write any regex here you like. If you're working with [LUIS dialogs](https://github.com/MSFTAuDX/BotBoiler/blob/master/src/dialogs/samples/luisDialog.ts), the trigger will be a regular ```string``` that matches the LUIS intent name. 

There are two steps, each one matches an available signature in the ```IDialogWaterfallStep``` interface from ```bot builder```. 

The steps are exposed via the ```get waterfall()``` accessor. The order they are listed here is the order they will "waterfall" during a conversation. 

#### Add some chat

Inside ```step1``` let's add some chat: 

```typescript
session.send(`Hi there! I'm boiler bot`);
builder.Prompts.text(session, `What's your name?`);    
```

Inside ```step2``` let's show what they entered:

```typescript
session.endConversation(`Welcome, ${results.response}`);
```

#### Add the dialog to the bot

Open up ```dialogIndex.ts``` and add: 

```typescript
import someDialog from './someDialog'
export {someDialog};
//it might look more like this now: export {someBasicDialog, luisDialog, qnaDialog, someDialog};
```

That's it! The system will now automatically add this dialog and you can now type "somedialog" in the emulator to kick things off (hopefully you're using the development flows listed above to automatically build and run!)

<img width="640" src="https://user-images.githubusercontent.com/5225782/27893138-2d78973a-6247-11e7-9a97-12d1a7965557.gif"/>

This is a super simple example. Let's get more complex. 

### Create an Injectable Component

A core tenet to this system is separation of concerns. To this end we're careful to always make sure a class does a single thing. A dialog is a dialog, it doesn't do other things like communicate to servers. 

### QnA Maker Component

In this example we'll add a component that can talk to the [Microsoft QnA Maker](https://docs.microsoft.com/en-au/azure/cognitive-services/qnamaker/home) Cognitive Service. QnA Maker is an AI service that makes it really simple to add basic Question and Answer functionality to your systems - including bots. 

You'll need to set one of these up before you can follow along here. If you don't want to set up QnA Maker now it's not a problem - this is just going to make a restful call to a web service, you can substitute the QnA Maker with any service. 

You'll need two values from your QnA Service - both are found on the settings page.  The knowledgebase id is found in the POST url example field: ```POST /knowledgebases/<knowledgebaseid>/generateAnswer```. Place this in the .env file ```QNA_ID``` field. 

The other value is the subscription key. This is foung in the same example area ```Ocp-Apim-Subscription-Key: <yourkey>```. Place this in the .env file ```QNA_SUBS_KEY``` field. 

Once your new QnA Maker is registered place the ```QNA_SUBS_KEY``` field in the ```.env``` file. 

### Create the component

It's suggested that you place your own components and services under src/model/components. 

Create a new file called ```qnaComponent.ts``` under src/model/components. The base template is similar to the dialog created earlier: 

```typescript
import * as builder from 'botbuilder';
import { injectable, inject } from "inversify";

import { serviceBase } from './../../system/services/serviceBase';
import * as contracts  from '../../system/contract/contracts';

@injectable()
export default class qnaComponent extends serviceBase {
   
}
```

Next you'll need an interface that can be registered with the dependency injection container. This is done in ```model/modelContracts.ts```.

Add the following interfaces:

 ```typescript
export interface IQnAAnswer {
    answer: string;
    score: number;
}

export interface IQnaComponent{
    getAnswer(question:string):Promise<IQnAAnswer>;
}

```

You'll also need to add a ```Symbol``` here to assist with depdency resolution at runtime. Unfortunately in Typescript, interfaces are not available at runtime, so we cannot use them to resolve depdencies at runtime, we must use a ```Symbol``` instead. 

Local ```let modelSymbols = {``` section and add a new Symbol for our new component:

```typescript
let modelSymbols = {
    IQnaComponent: Symbol("IQnaComponent")
}
```
In a typical project you'll end up with lots of these Symbols. See [this file](https://github.com/MSFTAuDX/BotBoiler/blob/master/src/system/contract/contracts.ts) for an example of how that would look. 

Now add the interface to your new component back in ```qnaComponent.ts```

First, include ```modelContracts``` by adding the following to the top of the file:

```typescript
import * as modelContracts from '../modelContracts';
```

Now implement from the interface:

```typescript
@injectable()
export default class qnaComponent extends serviceBase implements modelContracts.IQnaComponent {   
}
```

You may notice that VS Code as placed a little red underline under the class name ```qnaComponent```. You can click on this word, press ```ctrl-.``` and it will allow you to implement the interface. If you can't do that for some reason just paste in the following inside the class squigglies. 

```typescript
getAnswer(question: string): Promise<modelContracts.IQnAAnswer> {
    throw new Error("Method not implemented.");
}
```

Now we need to do the heavy lifting - actually call to get the QnA stuff. 

The core code of the framework includes a helper called [netClient](https://github.com/MSFTAuDX/BotBoiler/blob/master/src/system/helpers/netClient.ts) which can be injected in to any class at runtime. Let's inject that now. 

Create a new private field to host the ```netClient``` instance:

```typescript
private _netClient:contracts.INetClient
```

Next, request the ```netClient``` be injected:

```typescript
constructor(@inject(contracts.contractSymbols.INetClient) netClient:contracts.INetClient) {
    super();
    this._netClient = netClient;
}
```
**Note:** If the injection syntax seems a little strange to you (say for instance you're accustomed to Autofac) then you can brush up on [Inversify here](http://inversify.io/). 

#### Do some work - grab the QnA answer

First, add the ```async``` keyword on to the front of the ```getAnswer``` class:

```typescript
async getAnswer(question: string): Promise<modelContracts.IQnAAnswer> {
```

Find the line ```throw new Error("Method not implemented.");``` and replace it so your method looks like this:

```typescript
async getAnswer(question: string): Promise<modelContracts.IQnAAnswer> {        
    const bodyText = { question: question };
    const url = `https://westus.api.cognitive.microsoft.com`;
    const path = `/qnamaker/v1.0/knowledgebases/${this.config.qna_id}/generateAnswer`;
    
    this.logger.log(`Url: ${url}/${path} Question: ${question}`);


    try{
        var result = await this._netClient.postJson<any, modelContracts.IQnAAnswer>(url, path, bodyText,  
            {'Ocp-Apim-Subscription-Key':this.config.qna_subs}); 
        return result;
    }catch(e){
        return e;
    }
}
```
You can see the ```netClient``` being used to get the QnA result. You may also note it's using config from the base class ```serviceBase```. This is handy so you don't have to inject ```IConfig``` every time you want to use it somewhere. 

#### Register the new component for injection

In the above example, we were able to request that INetClient be injected in to the class because it has been registered on the IOC container for Dependency Injection. Now we need to do this for the new component. 

Open up ```startup.ts```. This is where all the DI binding happens. It's a magical place. 

Include a reference to your new component up the top. 

```typescript
//modelContracts is probably already there, add your new component after this line
import * as modelContracts from './model/modelContracts';
import qnaComponent from './model/components/qnaComponent';
```

Now locate the line in ```startup.ts``` that says "//Your services registered here". It's in ```_registerCustomComponents()```.

Add a new registration for your service:

```typescript
this.container.bind<modelContracts.IQnaComponent>(modelContracts.modelSymbols.IQnaComponent)
    .to(qnaComponent);
```

You can now request this to be injected in to a class such as a dialog!

#### Consume the new component

We'll consume the component from the sample dialog created before. Open ```someDialog.ts``` and paste in just under the other public fields:

```typescript
private _qnaMaker: modelContracts.IQnaComponent; 

constructor(@inject(modelContracts.modelSymbols.IQnaComponent)qnaMaker: modelContracts.IQnaComponent) {
    super();
    
    this._qnaMaker = qnaMaker;
}
```

You like like to change the trigger regex to something better like ```public trigger:RegExp = /^ask$/i```. 

Next change ```step1``` so it asks a question like "What would you like to know?"

```typescript
 step1(session: builder.Session, args:any, next:Function)  { 
    session.send(`Hi there! I'm boiler bot`);
    builder.Prompts.text(session, `What would you like to know?`);       
}
```

Next, make ```step2``` ```async``` and change the code to:

```typescript
 async step2(session: builder.Session, results:builder.IDialogResult<string>, next:Function) {  
    var question = results.response;
    try{
        var result = await this._qnaMaker.getAnswer(question);
        if (result.score > 50) {
            session.endConversation(result.answer);
        } else if (result.score > 0) {
            session.send(`I'm not sure if this is right, but here's what I know...`);
            session.endConversation(result.answer);
        } else {
            session.endConversation(`I don't have that answer.`);
        }
    }catch(e)
    {
        session.endConversation(`Alas, I am broken.`);
        this.logger.log(e);
    }  
}
```

Now you can type "ask" to your bot, then type a question from your QnA!

<img width="640" src="https://user-images.githubusercontent.com/5225782/27895544-c2a66328-6256-11e7-93ea-d44acb5bf3ff.gif"/>

Full listings of these classes can be found here: [qnaDialog](https://github.com/MSFTAuDX/BotBoiler/blob/master/src/dialogs/samples/qnaDialog.ts) and [qnaComponent](https://github.com/MSFTAuDX/BotBoiler/blob/master/src/model/comonents/samples/qnaComponent.ts)

## Links

- [BotBoiler on GitHub](https://github.com/MSFTAuDX/BotBoiler)
- [Getting started with the Bot Builder and the Bot Framework](https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-quickstart)
- [Manage conversation flow with dialogs](https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-dialog-manage-conversation-flow)
- [Getting started with the Bot Framework Emulator](https://docs.microsoft.com/en-us/bot-framework/debug-bots-emulator)
    - [Bot Framework Emulator Downloads](https://github.com/Microsoft/BotFramework-Emulator/releases)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Visual Studio code and Typescript](https://code.visualstudio.com/docs/languages/typescript)
- [Ava Javascript Unit Testing Framework](https://github.com/avajs/ava)
- [Microsoft QnA Maker](https://docs.microsoft.com/en-au/azure/cognitive-services/qnamaker/home)
