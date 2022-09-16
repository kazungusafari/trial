---
sidebar_position: 1
---

# Getting Started?

## How to install

```bash
  npm i @react/formfield
```

## Example

The following code excerpt demonstrates a basic usage example.

```js
import { Form, FormField  } from "@formfield/react";

export default function Example() {

  const onSubmit = (data) => {
    console.log(data)
  };

  return (
      <Form
       onSubmit={onSubmit}
       initialValues={{
         username: "",
         email: "",
         }}
         >
        <FormField
          name="email"
         >
          {({ getFieldProps}) => (
            <label>
              Email:
              <input
                {...getFieldProps()}
                type="email"
              />
            </label>
          )}
        </FormField>
        <FormField
          name="username"
        >
          {({ getFieldProps }) => (
            <label>
              Username:
              <input
                {...getFieldProps()}
                type="text"
              />
        </FormField>
        <button type="submit">
          Sign Up
        </button>
      </Form>
    )}



```

## Form Component

When managing form state,two of things we usually do is;

- Define the **initial state**.The initial values for the form
- Define **submission handler**.The function to be called when form is submitted

You can define these two things as `props of Form component` as shown below.

```js
import { Form, FormField } from "@formfield/react";

export default function Example() {
	const onSubmit = (data) => {
		console.log(data);
	};

	const initialValues = {
		username: "",
		email: "",
	};

	return (
		<Form onSubmit={onSubmit} initialValues={initialValues}>
			/* pass children here */
		</Form>
	);
}
```

## FormField Component and its Goodies

The FormField must wrap your form fields.`It takes the name and validation rules of a form field as props` and `share the following goodies to that form field using a variety of React patterns`.

- fieldHasErrors.This is a boolean value which says whether a form field has errors or not as the user is typing
- fieldErrors.This is a string containing the error for a form field as the user is typing.
- fieldValue.The value of the form field
- fieldName.The name of the form field
- fieldHandler.This is an onchange function handler
- getFieldProps .This is function that returns the object shown below

```js
const obj = { value: fieldValue, onChange: fieldHandler, name: fieldName };
```

Our basic form has two form fields for email and username respectively.We have to wrap each of these form fields using FormField component as shown below.

```js
import { Form, FormField } from "@formfield/react";

export default function Example() {
	const onSubmit = (data) => {
		console.log(data);
	};

	const initialValues = {
		username: "",
		email: "",
	};

	return (
		<Form onSubmit={onSubmit} initialValues={initialValues}>
			<FormField name="username">/* pass child here */</FormField>
			<FormField name="email">/* pass child here */</FormField>
		</Form>
	);
}
```

So how does FormField share its goodies with its child ? There are a variety of ways but today we will talk how FormField uses **render props pattern** to do this.

### Passing callback function as child of FormField

When `using render props pattern,you pass a callback function to the FormField` inside a curly braces as shown below.

> Javascript callbacks are expressions so you have put them in a curly braces when used together with JSX.

```js
import { Form, FormField } from "@formfield/react";

export default function Example() {
	const onSubmit = (data) => {
		console.log(data);
	};

	const initialValues = {
		username: "",
		email: "",
	};

	return (
		<Form onSubmit={onSubmit} initialValues={initialValues}>
			<FormField name="username">{() => {}}</FormField>
			<FormField name="email">{() => {}}</FormField>
		</Form>
	);
}
```

### What does this callback take ?

It takes the known goodies from FormField.For example one of the goodies is `getFieldProps`

```js
import { Form, FormField } from "@formfield/react";

export default function Example() {
	const onSubmit = (data) => {
		console.log(data);
	};

	const initialValues = {
		username: "",
		email: "",
	};

	return (
		<Form onSubmit={onSubmit} initialValues={initialValues}>
			<FormField name="username">{({ getFieldProps }) => {}}</FormField>
			<FormField name="email">{({ getFieldProps }) => {}}</FormField>
		</Form>
	);
}
```

### What does this callback return ?

It should return the form element that will be visible to the user.

```js
import { Form, FormField } from "@formfield/react";

export default function Example() {
	const onSubmit = (data) => {
		console.log(data);
	};

	const initialValues = {
		username: "",
		email: "",
	};

	return (
		 <Form
          onSubmit={onSubmit}
          initialValues={initialValues}
         >
        <FormField
          name="email"
         >
          {({ getFieldProps}) => (
            <label>
              Email:
              <input
                type="email"
              />
            </label>
          )}
        </FormField>
        <FormField
          name="username"
        >
          {({ getFieldProps }) => (
            <label>
              Username:
              <input
                type="text"
              />
			 </label>
        </FormField>
        <button type="submit">
          Sign Up
        </button>
      </Form>
	);
}
```

## getFieldProps

This a function when called return an object with following properties

```

    name : name of the field
    value : value of the field
    onChange : callback function to be called when the field value changes

```

You can spread the object as shown below to pass the above fields to form elements.

```javascript

import { Form, FormField } from "@formfield/react";

export default function Example() {
	const onSubmit = (data) => {
		console.log(data);
	};

	const initialValues = {
		username: "",
		email: "",
	};

	return (
		 <Form
          onSubmit={onSubmit}
          initialValues={initialValues}
         >
        <FormField
          name="email"
         >
          {({ getFieldProps}) => (
            <label>
              Email:
              <input
                {...getFieldProps()}
                type="email"
              />
            </label>
          )}
        </FormField>
        <FormField
          name="username"
        >
          {({ getFieldProps }) => (
            <label>
              Username:
              <input
               {...getFieldProps()}
                type="text"
              />
			 </label>
        </FormField>
        <button type="submit">
          Sign Up
        </button>
      </Form>
	);
}

```

## Apply Validation

We want the following validation

- All fields are required
- Email should be a valid email
- Minimum length length of username is four characters

The FormField also has a prop called `rules` that we can use to define the validation rules

```javascript

import { Form, FormField } from "@formfield/react";

export default function Example() {
	const onSubmit = (data) => {
		console.log(data);
	};

	const initialValues = {
		username: "",
		email: "",
	};

	return (
		 <Form
          onSubmit={onSubmit}
          initialValues={initialValues}
         >
        <FormField
          name="email"
          rules={[
            FormRules.required('Email is required'),
            FormRules.email("Provide a correct email")]}
         >
          {({ getFieldProps}) => (
            <label>
              Email:
              <input
                {...getFieldProps()}
                type="email"
              />
            </label>
          )}
        </FormField>
        <FormField
          name="username"
          rules ={[
            FormRules.required('Username is required')
            FormRules.minLength(4,'Minimum length of username characters is four')
            ]}
        >
          {({ getFieldProps }) => (
            <label>
              Username:
              <input
               {...getFieldProps()}
                type="text"
              />
			 </label>)}
        </FormField>
        <button type="submit">
          Sign Up
        </button>
      </Form>
	);
}

```

## Handle Errors

We want to show form element's error if it has any error

```javascript

import { Form, FormField,FormRules } from "@formfield/react";

export default function Example() {
  const onSubmit = (data) => {
    console.log(data);
  };

  const initialValues = {
    username: "",
    email: ""
  };

  return (
    <Form onSubmit={onSubmit} initialValues={initialValues}>
      <FormField
        name="email"
        rules={[
          FormRules.required("Email is required"),
          FormRules.email("Provide a correct email")
        ]}
      >
        {({ getFieldProps, fieldHasErrors, fieldErrors }) => (
          <>
            <label>
              Email:
              <input {...getFieldProps()} type="email" />
            </label>
            {fieldHasErrors && <span>{fieldErrors}</span>}
          </>
        )}
      </FormField>
      <FormField
        name="username"
        rules={[
          FormRules.required("Username is required"),
          FormRules.minLength(
            4,
            "Minimum length of username characters is four"
          )
        ]}
      >
        {({ getFieldProps, fieldHasErrors, fieldErrors }) => (
          <>
            <label>
              Username:
              <input {...getFieldProps()} type="text" />
            </label>
            {fieldHasErrors && <span>{fieldErrors}</span>}
          </>
        )}
      </FormField>
      <button type="submit">Sign Up</button>
    </Form>
  );
}

```
