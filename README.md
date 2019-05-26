# vue-a11y-input

A component for text-like inputs. Built with accessibility in mind.

## Installation

You can install the componnent using NPM or Yarn.

```
npm install vue-a11y-input --save
```

If you use Yarn:

```
yarn add vue-a11y-input
```

Once the component is installed you need to import wherever you want to use it.

```js
import A11yInput from 'vue-a11y-input'
```

Don’t forget to add it to the registered components (been there, done that):

```js
components: {
  A11yInput,
  // ... all the other amazing components
}
```

## API

This is just a quick overview. For an in-depth guide how to use the component check the section Usage below.

### Props

- `value`: The value shown inside of the input.
- `label`: The label text of the input. Required.
- `validation`: A vuelidate-like object, must contain properties named `$error` and `$dirty`. Required.
- `description`: Descriptive text giving a user more context on what form their input has to have. Optional.

### Styles

The component exposes the following CSS classes for its parts:

| Classname                   | Element                             |
| --------------------------- | ----------------------------------- |
| v-a11y-input                | Root                                |
| v-a11y-input\_\_input       | Text field                          |
| v-a11y-input\_\_description | Descriptive text                    |
| v-a11y-input\_\_feedback    | Container to show feedback messages |

By default no styles will be attached to these classes.

## Usage

This component was built to accept all text-like input formats. These are, for example, `password`, `email` or `date`.

Attributes you add when adding the component to your template are bound to the input element.

Say you want to add a password input to a form. If you add `type="password"` the component will take the type and apply it to the input element.

```html
<a11y-input type="password" />
```

`vue-a11y-input` is [v-model](https://vuejs.org/v2/guide/forms.html) compliant.

```html
<a11y-input v-model="password" type="password" name="password" />
```

Ths will result in the following input:

```html
<input
  id="6ac26f8f-930c-4dc4-a098-b00094b56906"
  aria-invalid="false"
  type="password"
  name="password"
  class="v-a11y-input__input"
/>
```

💁‍ _Note:_ You do not need to pass in a `id`. A unique ID for every instance of the component is automatically generated.

### Label

Input elements [must have a linked label](https://www.w3.org/TR/WCAG20-TECHS/H44.html) to give the input an accessible name.

To do so, pass in the `label` prop when using the component.

```html
<a11y-input
  v-model="password"
  type="password"
  name="password"
  label="Your password"
/>
```

💁 _Note:_ `label` is required.

### Description

Sometimes it is useful to describe expected conditions. For example, a user has to enter a password that is at least eight characters long.

To add a description, pass in the prop named, you might have guessed it, `description`.

```html
<a11y-input
  v-model="password"
  type="password"
  name="password"
  label="Your password"
  description="Your password has to be at least eight characters long."
/>
```

This will render the description in a `p` element which is linked to the `input` via the [`aria-describedby` attribute](https://www.w3.org/TR/WCAG20-TECHS/ARIA1.html).

### Required inputs

In addition to binding the `required` attribute on the input element the component exposes a slot inside of its `label` element to add a visual clue that the user has to fill in data.

Bear in mind that the popular \* might not be enough to indicate a required field. For further reading I recommend the article [Required Fields](https://a11y-101.com/development/required) on a11y-101.com

```html
<a11y-input
  v-model="password"
  :required="true"
  type="password"
  name="password"
  label="Your password"
  description="Your password has to be at least eight characters long."
>
  <template v-slot:required-text>
    <span class="aside">required</span>
  </template>
</a11y-input>
```

💁 _Note:_ This example uses the named slot syntax introduced in Vue 2.6. [Take a look in the docs](https://vuejs.org/v2/guide/components-slots.html#Named-Slots) for usage examples and how to use named slots in versions prior to 2.6.

### Validation

No input without validation, right?

You will have to take care of this yourself, though. The component can and should not know what value is expected inside of it.

Nonetheless, I tried to make it as easy as possible to use the component along existing solutions like [Vuelidate](https://vuelidate.netlify.com/).

In fact, if you are already using Vuelidate, you are good to go.

`vue-a11y-input` expects a vuelidate-like validation object. Namely the properties `$error` and `$dirty`.

For our password example the Vuelidate config might look something like this:

```js
import { required, minLength } from 'vuelidate/lib/validators'

export default {
  // Component context omitted for brevity
  validations: {
    password: {
      required,
      minLength: minLength(8)
    }
  }
}
```

You can use `$v.password` as the prop for the input component without the need to change anything.

```html
<a11y-input
  v-model="password"
  :required="true"
  :validation="$v.password"
  type="password"
  name="password"
  label="Your password"
  description="Your password has to be at least eight characters long."
>
  <template v-slot:required-text>
    <span class="aside">required</span>
  </template>
</a11y-input>
```

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

```
yarn run serve
```

### Compiles and minifies for production

```
yarn run build
```

### Run your tests

```
yarn run test
```

### Lints and fixes files

```
yarn run lint
```

### Run your unit tests

```
yarn run test:unit
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
