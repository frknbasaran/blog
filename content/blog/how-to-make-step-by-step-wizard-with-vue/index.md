---
title: How to make step-by-step wizard with Vue?
date: "2021-05-29T17:41:00.000Z"
description: We're familiar to step-by-step user interfaces from various modern web applications in these days. In this article I'll tell to you how you can implement step by step wizard structure to your web apps easily with vue-wizard-steps package.
---

I'll tell to you how you can implement step by step wizard structure to your web apps easily with **vue-wizard-steps** package in this article . We're familiar to step-by-step user interfaces from various modern web applications in these days. 

### **Let's start!**

Let's assume we'll create a profile view that require some informations like *contact informations*, *security questions* and *profile picture* from user. We can separate these to three different steps like **Contact**, **Security** and **Profile**.

<img src="/draw.png" width="100%" alt="representive drawing for our wizard steps">

As you can see we'll have a progress bar that contain three different steps and step titles.

### **Time to code!**

I'll use to **vue-cli** for creating clean vue application in this article but of course you can select any other way for creating your project or use this package in your existing projects.

**Step 1: Install Vue Cli**

```bash
yarn global add @vue/cli
```

*My vue-cli version was 4.5.12 when the date I wrote this article.*

**Step 2: Create clean Vue app**

```bash
vue create wizard
```

*This command may require some approves during process.*

**Step 3: Install Vue Wizard Steps package**

After vue app generation, you need to go project directory and run this command on project root.

```bash
yarn add vue-wizard-steps
```

Now we're ready to code.

![Project structure after vue-cli generation process](/structure.png)

Your project folder structure probably looks like above. 

**Step 4: Starting implementation**

As first, clear the content of App.vue file and create step-by-step, add vue-wizard-steps package as dependency into view, and register it as component.

```javascript
<script>
import WizardSteps from 'vue-wizard-steps';

export default {
  components: {
    WizardSteps
  }
}
</script>
```

And then, we need a data model for manage wizard. Let's add sections property to data() function.

```javascript
<script>
import WizardSteps from 'vue-wizard-steps';

export default {
  components: {
    WizardSteps
  },
  data() {
    return {
      sections: {
        // titles array for step
        titles:  ['contact', 'security', 'profile'],
        // filled step count as initially
        fillCount: 0
      },
      // optional background color 
      bgColor: '#E4F5B8',
      // optional filled step background color 
      fillColor: '#1F01B9'
    }
  }
}
</script>
```

Now it's time for changing our template. We're adding **WizardSteps** component with sections prop under to the wrapper div. 

```html
<template>
  <div id="app">
  	//wizard steps component
    <WizardSteps id="wizard" :sections="sections"/>
  </div>
</template>
```

Let's make some basic style improvements for our layout.

```css
<style>
#app {
  font-family: Helvetica, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
}
#wizard {
  width: 1024px;
}
</style>
```

On this point, we created a wizard/progress bar that contain three diferent steps as **Contact**, **Security** and **Profile** and centered it on the main layout. But it'd better if we add some example contents that changin by current step. Right?

Let's add some conditional contents and some navigation buttons to our template.

```html
<template>
  <div id="app">
    <WizardSteps id="wizard" :sections="sections"/>
    <div v-if="sections.fillCount === 0" class="content">
      <h1>Contact Informations</h1>
      <div class="navigation-buttons">
        <button @click="sections.fillCount = 1">next</button>
      </div>
    </div>
    <div v-if="sections.fillCount === 1" class="content">
      <h1>Security Informations</h1>
      <div class="navigation-buttons">
        <button @click="sections.fillCount = 0">previous</button>
        <button @click="sections.fillCount = 2">next</button>
      </div>
    </div>
    <div v-if="sections.fillCount === 2" class="content">
      <h1>Profile Informations</h1>
      <div class="navigation-buttons">
        <button @click="sections.fillCount = 1">previous</button>
        <button @click="sections.fillCount = 3">finish</button>
      </div>
    </div>
    <div v-if="sections.fillCount === 3" class="content">
      <h1>Finished!</h1>
    </div>
  </div>
</template>
```

As you can see we added four new div that has **.content** class name and all divs has navigation buttons inside of them. We're changing **sections.fillCount** variable for navigation between steps and also we're using it on conditional rendering with **v-if** directive.

With some final style changes we'll be ready to go.

```css
<style>
#app {
  font-family: Helvetica, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
}
#wizard {
  width: 1024px;
}
.content {
  width: 1024px;
}
.navigation-buttons {
  display: flex;
  justify-content: space-between;
}
</style>
```

That's all! We have three steps in our wizard and we have three different content area that appear depending current step. Also we have a finish message when appearing on final step.

You can access whole example project from [here](https://github.com/frknbasaran/vue-wizard-steps/tree/master/example). 