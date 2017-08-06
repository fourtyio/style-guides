# Super Awesome Ember Style Guide

- [General](#general)
- [Import Importance](#import-importance)
- [The Order of Things](#the-order-of-things)
- [Template Signature](#template-signature)

## General
#### Naming Convention


## Import Importance
When importing requirements it is important to limit it to only what is required. Ember recommends to import Ember directly: `import Ember from 'ember'` and then destructure from there
```javascript
import Ember from 'ember';
const {
  Controller,
  service: { inject }
} = Ember;
```
Now the above translates to:
```javascript
import Controller from 'ember-controller'
import service from 'ember-service/inject'
```
Another example to illustrate this is the _lodash library. Traditionally we would have use `import _ from 'npm:lodash'`. This is great.. but far from optimal, now we only import the specific method used. Examples:
```javascript
import forEachRight from 'npm:lodash/forEachRight';
import has from 'npm:lodash/has';
import lastIndexOf from 'npm:lodash/lastIndexOf';
...
```

## The Order of Things
The order in which properties, methods and actions are definded makes it easier for dev's to scan and reason about a particular file. Each type separated with a line break and within each props are ordered by name. Here are some rules to follow. (See [Import Importance](#import-importance))

1. Services.
2. Regular/Simple Properties; eg: `tagName`, `classNames`, `isModalOpen` ...
3. Single Line Computed Properties; eg: `alias`, `or`, `readOny` ...
4. Multi Line Computed Properties
5. Methods & Generator Functions; non-lifestyle hooks
6. Lifestyle Hooks
7. Actions
```javascript
export default Component.Extend({
  // Services
  auth: service(),
  websocket: service(),

  // Regular / Simple Props
  classNames, ['my-awesome-class'],
  tagName: 'li',

  // Single Line Computed Props
  channel: readOnly('session.currentChannel'),
  userId: alias('user.id'),

  // Multi Line Computed Props
  userName: computed('user.{firstName,lastName}', function () { 
    // do something
  }),

  // Methods & Generator Functions
  click(e) {
    e.stopPropagation();
    // do something
  },
  closeSocket() {
    // close the websocket
  },
  openSocket: task(function* () {
    // yield something
  }),

  // Lifestyle Hooks
  didReceiveAttrs() {
    this._super(...arguments);
    // more code
  },
  init() {
    this._super(...arguments);
    // more code    
  },
  willDestroyElement() {
    this._super(...arguments);    
    // more code    
  },

  actions: {
    handleClick() {
      // do something
    },
    updateUser(user) {
      // do something with user
    }
  },
});
```

## Template Signature
Template Signatures must use the following structure, including when only 1 prop is required. Props are ordered by name within each grouping. Lines use double indent.
1. Regular properties and arguments, **including `class`**
2. Action properties. All actions passed should use the `(action keyword)`
3. Yield reference 

n.b: classes ordering themselves may/may-not be in order.

#### Bad
```handlebars
{{#my-awesome-component propOne='hello world'
  class='some-awesome-class another-awesome-class
  propTwo=someProp
  propThree=third
  userClickAction=(action 'iLiveWithinThisComponent')
  someOtherAction=iAmAPassedInReference}}
  some code
{{/my-awesome-component}}
```
#### Good
```handlebars
{{#my-awesome-component
    class='some-awesome-class another-awesome-class'
    propOne='hello world'
    propThree=third
    propTwo=someProp
    someOtherAction=(action iAmAPassedInReference)
    userClickAction=(action 'iLiveWithinThisComponent')}}
  some code
{{/my-awesome-component}}
```
#### Good (yield)
```handlebars
{{#my-awesome-component
    class='some-awesome-class another-awesome-class'
    propOne='hello world'
    propThree=third
    propTwo=someProp
    someOtherAction=(action iAmAPassedInReference)
    userClickAction=(action 'iLiveWithinThisComponent')
    as |awesome|}} <-- yield reference is arbitrary, though should be appropriately named
  some code
{{/my-awesome-component}}
```
#### Bad / Only One Prop
Why is this bad? Following the same logic as trailing comma, github friendly
```handlebars
{{#my-awesome-component prop='someValue'}}
  some code
{{/my-awesome-component}}
```
#### Good / Only One Prop
```handlebars
{{#my-awesome-component 
    prop='someValue'}}
  some code
{{/my-awesome-component}}
```

### Template Action Reference
As noted in [Template Signature](#template-signature) all actions should be passed using the (action helper)
#### Bad
```handlebars
{{#my-awesome-component
  ...
  someAction=somePassedInAction}}
  some code
{{/my-awesome-component}}
```
#### Good
```handlebars
{{#my-awesome-component
    ...
    someAction=(action somePassedInAction)}}
  some code
{{/my-awesome-component}}
```

### Todo
- organization (order of things)
  - models
- full section on naming
  - bool props eg: isX or hasY
  - 
