# VPhoneInput

[![Tests](https://github.com/paul-thebaud/v-phone-input/actions/workflows/tests.yml/badge.svg)](https://github.com/paul-thebaud/v-phone-input/actions/workflows/tests.yml)
[![Publish](https://github.com/paul-thebaud/v-phone-input/actions/workflows/publish.yml/badge.svg)](https://github.com/paul-thebaud/v-phone-input/actions/workflows/publish.yml)
[![codecov](https://codecov.io/gh/paul-thebaud/v-phone-input/branch/main/graph/badge.svg?token=75BHV1FRQ9)](https://codecov.io/gh/paul-thebaud/v-phone-input)
[![NPM version](https://img.shields.io/npm/v/v-phone-input)](https://www.npmjs.com/package/v-phone-input)
![NPM downloads](https://img.shields.io/npm/dt/v-phone-input)
![MIT license](https://img.shields.io/npm/l/v-phone-input)

> [Check out the next version with Vue 3 and Vuetify 3 compatibility](https://github.com/paul-thebaud/v-phone-input/tree/next)

International phone field for [Vuetify 2.0](http://vuetifyjs.com) and
[Vue JS 2](https://vuejs.org/).

- Simple and standardized value using E164 formatted phone numbers (example: +33123456789)
- [Searchable countries](#enabling-searching-countries)
- [Automatic validation](#validation)
- [Customizable display format](#customizing-display-format)
- [Easy localization with label functions](#localization)
- [Customizable countries icons](#country-icon-modes)
- [Relies on external packages](#dependencies) to provide countries data and icons
- Built with Typescript, fully unit/end-to-end tested

Version 2 with better support for ES Modules, provided by Vite, is now released. Check
out [the migration guide](#migrate-from-1xx-to-2xx).

Proudly supported by the [CoWork'HIT](https://coworkhit.com).

**Motivation:** There are already multiple libraries to provide phone number input on Vuetify. But
for those already published are not actively maintained or does not put focus on providing great
accessibility. This new library aims to provide those two.

## Demo

You can try the VPhoneInput with major options configuration on
the [GitHub pages demo](https://paul-thebaud.github.io/v-phone-input/).

On the demo, you'll be able to try out the field option and generate your plugin registration
options.

## TL;DR

Installation though Yarn:

```shell
yarn add v-phone-input@latest flag-icons
```

Installation though NPM:

```shell
npm install v-phone-input@latest flag-icons
```

Plugin installation:

```javascript
import Vue from 'vue';
import { createVPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';
import 'flag-icons/css/flag-icons.min.css';

const vPhoneInput = createVPhoneInput({
  countryIconMode: 'svg',
});

Vue.use(vPhoneInput);
```

Component usage:

```vue

<template>
  <v-phone-input v-model="phone" />
</template>

<script>
export default {
  data: () => ({ phone: '' }),
};
</script>
```

## Documentation

### Table of contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Migration](#migration)
- [Usage](#usage)
- [Props](#props)
- [Events](#events)
- [Slots](#slots)
- [Examples](#examples)
    - [Country icon modes](#country-icon-modes)
    - [Validation](#validation)
    - [Enabling searching countries](#enabling-searching-countries)
    - [Customizing display format](#customizing-display-format)
    - [Localization](#localization)
- [Dependencies](#dependencies)
- [Types](#types)
    - [Country ISO-2](#country-iso-2)
    - [Country](#country)
    - [Country guesser](#country-guesser)
    - [Phone number formats](#phone-number-formats)
    - [Phone number](#phone-number)
    - [Message options](#message-options)
    - [Message](#message)
    - [Message resolver](#message-resolver)

### Requirements

VPhoneInput requires `Vue@2` and `Vuetify@2` to be installed and working in your project.

> VPhoneInput utilizes features of ES2015/2017 that require the need to use polyfills for
> Internet Explorer 11 and Safari 9/10.

> [Available country guessers](#country-guesser) requires fetch.

### Installation

You can install this package though Yarn:

```shell
yarn add v-phone-input@latest flag-icons
```

Or NPM:

```shell
npm install v-phone-input@latest flag-icons
```

> `flag-icons` package is required if you want the input to display countries' flags.

### Usage

You can globally define the input using the provided plugin factory. This will register
the `v-phone-input` component globally. You must also import the package additional CSS.

```javascript
import Vue from 'vue';
import { createVPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';
import 'flag-icons/css/flag-icons.min.css';

const vPhoneInput = createVPhoneInput({
  countryIconMode: 'svg',
});

Vue.use(vPhoneInput);
```

You may also only define the field on a per-file basis. Notice that with this method, you won't be
able to define props' default values for the input.

```vue

<template>
  <v-phone-input
      v-model="phone"
      country-icon-mode="svg"
  />
</template>

<script>
import { VPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';
import 'flag-icons/css/flag-icons.min.css';

export default {
  components: { VPhoneInput },
  data: () => ({
    // Phone value will be a E164 formatted phone number (example: +33123456789).
    phone: '',
  }),
};
</script>
```

### Migration

#### Migrate from 1.x.x to 2.x.x

Upgrade the package version with Yarn:

```shell
yarn upgrade v-phone-input@^2.0.0
```

If you are using the plugin the global plugin registration method, you need to update the file where
you are defining it:

```diff
  import Vue from 'vue';
- import VPhoneInputPlugin from 'v-phone-input';
+ import { createVPhoneInput } from 'v-phone-input';
+ import 'v-phone-input/dist/v-phone-input.css';
  import 'flag-icons/css/flag-icons.min.css';

- Vue.use(VPhoneInputPlugin, { countryIconMode: 'svg' });
+ const vPhoneInput = createVPhoneInput({ countryIconMode: 'svg' });
+ Vue.use(vPhoneInput);
```

> CSS have been extracted to a file, so you'll also need to
> import `v-phone-input/dist/v-phone-input.css` (even if you are using the per-file registration),
> like in the example above.

### Props

VPhoneInput provides many props to customize the input behaviors or display.

You may pass those props directly the input:

```vue

<template>
  <v-phone-input label="Your Phone number" />
</template>
```

Or define them as default values when creating the plugin:

```javascript
import Vue from 'vue';
import { createVPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';

const vPhoneInput = createVPhoneInput({
  label: 'Your phone number',
});

Vue.use(vPhoneInput);
```

| Name                     | Type                                                               | Default                                                               | Description                                                                                                                       |
|--------------------------|--------------------------------------------------------------------|-----------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `label`                  | [`MessageResolver`](#message-resolver)                             | Phone                                                                 | The phone input label (see [Localization](#localization)).                                                                        |
| `ariaLabel`              | [`MessageResolver`](#message-resolver)                             | `undefined`                                                           | The phone input `aria-label` (see [Localization](#localization)).                                                                 |
| `countryLabel`           | [`MessageResolver`](#message-resolver)                             | Country                                                               | The country input label (see [Localization](#localization)).                                                                      |
| `countryAriaLabel`       | [`MessageResolver`](#message-resolver)                             | Country for {label}                                                   | The phone input `aria-label` (see [Localization](#localization)).                                                                 |
| `placeholder`            | [`MessageResolver`](#message-resolver)                             | `undefined`                                                           | The phone input placeholder (see [Localization](#localization)).                                                                  |
| `hint`                   | [`MessageResolver`](#message-resolver)                             | `undefined`                                                           | The phone input hint (see [Localization](#localization)).                                                                         |
| `invalidMessage`         | [`MessageResolver`](#message-resolver)                             | The "{label}" field is not a valid phone number (example: {example}). | The phone input message when number is invalid (see [Localization](#localization)).                                               |
| `countryIconMode`        | `string` or `VueConstructor` or `undefined`                        | `undefined`                                                           | The country icon display mode (see [Country icon modes](#country-icon-modes)).                                                    |
| `allCountries`           | [`Country[]`](#country)                                            | An array of all possible countries                                    | Array of countries to use.                                                                                                        |
| `preferredCountries`     | [`CountryIso2[]`](#country-iso-2)                                  | `[]`                                                                  | Array of countries' codes to propose as first option of country input.                                                            |
| `onlyCountries`          | [`CountryIso2[]`](#country-iso-2)                                  | `[]`                                                                  | Array of countries' codes to display as options of country input (will hide others).                                              |
| `ignoredCountries`       | [`CountryIso2[]`](#country-iso-2)                                  | `[]`                                                                  | Array of countries' codes to hide from country input.                                                                             |
| `defaultCountry`         | [`CountryIso2`](#country-iso-2)                                    | `undefined`                                                           | Default country to select when not guessing nor detecting from current value.                                                     |
| `countryGuesser`         | [`CountryGuesser` or `PreferableCountryGuesser`](#country-guesser) | `new MemoIp2cCountryGuesser()`                                        | Country guesser implementation to use when guessing country (see [Country guesser](#country-guesser)).                            |
| `disableGuessingCountry` | `boolean`                                                          | `false`                                                               | Disable guessing country.                                                                                                         |
| `disableGuessLoading`    | `boolean`                                                          | `false`                                                               | Disable passing the country input in a loading state when guessing country.                                                       |
| `enableSearchingCountry` | `boolean`                                                          | `false`                                                               | Turns the country input into a `VAutocomplete` input (see [Enabling searching countries example](#enabling-searching-countries)). |
| `rules`                  | `Function[]` or `string[]`                                         | `[]`                                                                  | Additional rules to pass to phone input (see [Validation example](#validation)).                                                  |
| `displayFormat`          | `PhoneNumberFormat`                                                | `'national'`                                                          | Format to use when displaying valid phone numbers in phone text input (see [Phone number formats](#phone-number-formats)).        |
| `country`                | `string`                                                           | `''`                                                                  | Country of the country input. Can be used with `.sync` modifier. Will be superseded by value's country if defined on mounting.    |
| `value`                  | `string`                                                           | `''`                                                                  | Value of the phone input. You may use it using `v-model` or `@input`.                                                             |
| `countryProps`           | `object`                                                           | `{}`                                                                  | Props to pass to the `VSelect` or `VAutocomplete` country input component.                                                        |
| `phoneProps`             | `object`                                                           | `{}`                                                                  | Props to pass to the `VTextField` phone input component.                                                                          |

> You can also pass the [Vuetify `VTextField`](https://vuetifyjs.com/en/api/v-text-field/#props)
> props to the component to customize display, errors, etc.

### Events

| Name             | Type                            | Description                                                                        |
|------------------|---------------------------------|------------------------------------------------------------------------------------|
| `input`          | `string`                        | Emitted when the country or phone is updated with the E164 formatted phone number. |
| `update:country` | [`CountryIso2`](#country-iso-2) | Emitted when the country is updated with the selected country.                     |

> All the [Vuetify `VTextField`](https://vuetifyjs.com/en/api/v-text-field/#props) events
> are also re-emitted by the input.

### Slots

The following slots are passed to the country select input (
see [v-select API](https://vuetifyjs.com/en/api/v-select/#slots)):

- `selection`
- `item`

The following slots are passed to the phone text input (
see [v-text-field API](https://vuetifyjs.com/en/api/v-text-field/#slots)):

- `append`
- `append-outer`
- `counter`
- `label`
- `message`
- `prepend`
- `prepend-inner`
- `progress`

The input also provides two special slots: the `country-icon` slot for countries' icons display
and `country-name` slot for countries' name display.

```vue

<template>
  <v-phone-input>
    <template #country-icon="{ country, decorative }">
      <img
          :src="`path/to/flags/${country.iso2}.png`"
          :alt="decorative ? '' : country.name"
      >
    </template>
    <template #country-name="{ country }">
      <strong>+{{ country.dialCode }}</strong>
      {{ country.name }}
    </template>
  </v-phone-input>
</template>
```

### Examples

#### Country icon modes

With VPhoneInput, you can choose between 5 country icon modes which are changing the way the country
input will display.

##### SVG

This is the proposed way to use the input. Rely on an SVG flag icons package. You must
install [`flag-icons`](https://www.npmjs.com/package/flag-icons) package to use it.

```javascript
import Vue from 'vue';
import { createVPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';
import 'flag-icons/css/flag-icons.min.css';

const vPhoneInput = createVPhoneInput({
  countryIconMode: 'svg',
});

Vue.use(vPhoneInput);
```

##### Sprite

Rely on a CSS sprite flag icons package. You must
install [`world-flags-sprite`](https://www.npmjs.com/package/world-flags-sprite) package to use it.

```javascript
import Vue from 'vue';
import { vPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';
import 'world-flags-sprite/stylesheets/flags32.css';

const vPhoneInput = createVPhoneInput({
  countryIconMode: 'sprite',
});

Vue.use(vPhoneInput);
```

##### Custom component

This allows you to register a custom component to display country icons. Component will
receive `country` and `decorative` props. We provide a simple `VPhoneCountrySpan` component to
simplify using a CSS class image based icon system (such as another CSS sprite file).

```javascript
import Vue from 'vue';
import { createVPhoneInput, VPhoneCountrySpan } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';
import 'your/awesome/flag-sprite.css';

const vPhoneInput = createVPhoneInput({
  countryIconMode: {
    functional: true,
    render: (h, { props }) => h(VPhoneCountrySpan, {
      staticClass: `awesome-flag awesome-flag-${props.country.iso2.toLowerCase()}`,
      props,
    }),
  },
});

Vue.use(vPhoneInput);
```

##### Custom slot

See the [slots section](#slots).

##### No icon

This is the default behavior when not overriding options or props default values. This will not
display an icon inside the list, but will show the ISO-2 code inside the selection for screen
readers users.

#### Validation

By default , the input will validate that the phone number is a valid one by injecting a rules to
the phone text input.

You may add any additional rules by providing a `rules` prop to the input:

```vue

<template>
  <v-phone-input :rules="rules" />
</template>

<script>
export default {
  computed: {
    rules() {
      return [
        (value, phone, { label, example }) => !!value || `The "${label}" field is required.`,
      ];
    },
  },
};
</script>
```

Any rule you pass as a function will receive 3 arguments (instead of one for default Vuetify rules)
that you may use when validating user's input:

- `value`: the value contained in the phone text input.
- `phone`: the [phone number object](#phone-number).
- `messageOptions`: the [message options](#message-options) which you may use to inject the input
  label or a phone example inside the message.

> If you don't want the automatic validation to run, you can pass a `null` value to the
> `invalid-message` prop.

#### Enabling searching countries

You may provide a `enableSearchingCountry` with a `true` value to enable textual search in
countries.

> Since VPhoneInput does not import VAutocomplete to reduce its weight, you might need to provide
> this component to Vue when treeshaking Vuetify components
> (e.g. when using `vuetify-loader`).

To enable searching countries for all inputs as a default behavior:

```javascript
import Vue from 'vue';
import { VAutocomplete } from 'vuetify/lib';
import { createVPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';

// IMPORTANT: required when treeshaking Vuetify components.
Vue.component('VAutocomplete', VAutocomplete);

const vPhoneInput = createVPhoneInput({
  enableSearchingCountry: true,
});

Vue.use(vPhoneInput);
```

To enable searching countries on a per-input basis:

```vue

<template>
  <v-phone-input enable-searching-country />
</template>

<script>
import { VAutocomplete } from 'vuetify/lib';

export default {
  // IMPORTANT: required when treeshaking Vuetify components.
  components: { VAutocomplete },
};
</script>
```

#### Customizing display format

By default, valid phone number will be formatted using the national format. You can customize the
display format using the `displayFormat` prop/option:

```javascript
import Vue from 'vue';
import { createVPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';

const vPhoneInput = createVPhoneInput({
  displayFormat: 'international',
});

Vue.use(vPhoneInput);
```

#### Localization

Localizable props may be defined on a per-input basis:

```vue

<template>
  <v-phone-input
      label="Phone number"
      country-label="Country"
      country-aria-label="Country for phone number"
      invalid-message="Phone number must be a valid phone number (example: 01 23 45 67 89)."
  />
</template>
```

Localizable props can also be defined for all inputs as a default behavior:

```javascript
import Vue from 'vue';
import { createVPhoneInput } from 'v-phone-input';
import 'v-phone-input/dist/v-phone-input.css';

// Example without any localization library.
const vPhoneInput = createVPhoneInput({
  label: 'Phone number',
  countryLabel: 'Country',
  countryAriaLabel: ({ label }) => `Country for ${label}`,
  invalidMessage: ({ label, example }) =>
    `${label} must be a valid phone number (example: ${example}).`,
});

// Example with Vue-I18N localization library.
import i18n from './path/to/i18n-plugin';

const vPhoneInput = createVPhoneInput({
  label: i18n.t('phone.phoneLabel'),
  countryLabel: i18n.t('phone.phoneCountry'),
  countryAriaLabel: (options) => i18n.t('phone.phoneCountryFor', options),
  invalidMessage: (options) => i18n.t('phone.invalidPhoneGiven', options),
});
```

> Any localizable prop is a [message resolver](#message-resolver). Notice that for `label`
> and `ariaLabel` props, no label will be defined for the message resolver's options.

> To disable a message, you should pass `null` (instead of `undefined`). This way, you'll be able
> to disable the country label for example (be sur to provide an explicit `countryAriaLabel`
> when doing so).

### Dependencies

VPhoneInput relies on multiple dependencies to work:

- [`awesome-phonenumber`](https://www.npmjs.com/package/awesome-phonenumber) for phone number
  formats, validation, etc.
- [`country-telephone-data`](https://www.npmjs.com/package/country-telephone-data) for phone number
  countries ISO-2, dial code and names.
- [`flag-icons`](https://www.npmjs.com/package/flag-icons) when using the `svg` country icon mode
  for SVG country flags.
- [`world-flags-sprite`](https://www.npmjs.com/package/world-flags-sprite) when using the `sprite`
  country icon mode for CSS sprite country flags.

### Types

#### Country ISO-2

A country ISO-2 code is a string containing 2 uppercase characters.

```typescript
type CountryIso2 = string;
```

#### Country

A country object contains information about a country.

```typescript
interface Country {
  name: string;       // Example: "France".
  iso2: CountryIso2;  // Example: "FR".
  dialCode: string;   // Example: "33".
}
```

#### Country Guesser

A country guesser is a class implementing `CountryGuesser` interface and providing a `guess` method
to detect the default country to use.

```typescript
interface CountryGuesser {
  guess: () => Promise<CountryIso2 | undefined>;
}
```

This package ships with two `CountryGuesser` implementations:

- `Ip2cCountryGuesser` which uses [IP2C service](https://ip2c.org) to guess the country from the
  client's IP. Notice that IP2C service might not work when using an add blocking extension.
- `MemoIp2cCountryGuesser` (default) which memoize the result of `Ip2cCountryGuesser` promise into a
  class property.
- `StorageMemoIp2cCountryGuesser` which memoize the result of `Ip2cCountryGuesser` promise into a
  storage implementation (defaults to `localStorage`).

A preferable country guesser is a country guesser with the capability to use the user preference
instead of the memoized guessed country.

```typescript
interface PreferableCountryGuesser extends CountryGuesser {
  setPreference: (country: CountryIso2) => void;
}
```

`MemoIp2cCountryGuesser` and `StorageMemoIp2cCountryGuesser` are implementations of
the `PreferableCountryGuesser` interface. They store the country (when changed using the input) to
return it instead of the guessed country on future `guess` call.

#### Phone Number Formats

A phone number format is a string representing a format, allowing to change the display format of a
phone number in input. Here are the available format (provided
by [awesome-phonenumber](https://www.npmjs.com/package/awesome-phonenumber)):

- `e164`: `+46707123456`
- `international`: `+46 70 712 34 56`
- `national`: `070-712 34 56`
- `rfc3966`: `tel:+46-70-712-34-56`
- `significant`: `707123456`

#### Phone Number

A phone number is a simple object describing the VPhoneInput current value. The `number` key will
contain an `input` string and may contain each formatted value if the phone number is valid.

```typescript
type PhoneNumberObject = {
  number: { input: string } & Partial<Record<PhoneNumberFormat, string>>;
  possibility: string;
  possible: boolean;
  valid: boolean;
  regionCode: CountryIso2 | undefined;
}
```

#### Message options

An object containing the input `label` (or `aria-label` if no label) and an example of a valid phone
number for active country.

```typescript
type MessageOptions = {
  label?: Message;
  example: string;
}
```

#### Message

A type representing a localized message for the input which will be used as the label, hint, etc.

```typescript
export type Message = string | undefined | null;
```

#### Message resolver

A type representing a function to resolve a message using current message options or directly the
message.

```typescript
export type MessageResolver = ((options: MessageOptions) => Message) | Message;
```

## Contributing

Please see [CONTRIBUTING file](CONTRIBUTING.md) for more details.

Informal discussion regarding bugs, new features, and implementation of existing features takes
place in the
[GitHub issue page of this repository](https://github.com/paul-thebaud/v-phone-input/issues).

## Credits

- [Paul Thébaud](https://github/paul-thebaud)
- [CoWork'HIT](https://coworkhit.com)
- [All Contributors](https://github.com/paul-thebaud/v-phone-input/graphs/contributors)

Inspired by [vue-tel-input](https://github.com/iamstevendao/vue-tel-input)
and [vue-tel-input-vuetify](https://github.com/yogakurniawan/vue-tel-input-vuetify). Library
skeleton generated by [vue-sfc-rollup](https://github.com/team-innovation/vue-sfc-rollup).

## License

`v-phone-input` is an open-sourced software licensed under the
[MIT license](https://opensource.org/licenses/MIT).
