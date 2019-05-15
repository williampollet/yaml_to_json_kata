# Yaml to JSON

## Context

A Ruby on Rails based company decides to change its translations engine.

The actual engine works with translations stored in `yaml` files, one file per locale:
```yaml
# en.yaml
en:
  admin:
    create:
      success: 'admin successfully created'
      failure: 'failure to create the admin'
  project:
    create:
    change_amount:
      success: 'the amount of your project has been modified'

# fr.yaml
fr:
  admin:
    create:
      success: 'administrateur créé avec succès'
      failure: 'administrateur n\'a pas pu être créé'
  project:
    create:
    change_amount:
      success: 'le montant de votre projet a été modifié'
  user:
    notify:
      account_changed: 'votre compte a bien été modifié'
```

It would like to switch to a `JSON` storage format, with concatenated keys:
```json
{
  "admin.create.success": {
    "fr": "administrateur créé avec succès",
    "en": "admin successfully created"
  },
  "admin.create.failure": {
    "fr": "administrateur n'a pas pu être créé",
    "en": "failure to create the admin"
  },
  "project.create": {
    "fr": null,
    "en": null
  },
  "project.change_amount.success": {
    "fr": "le montant de votre projet a été modifié",
    "en": "the amount of your project has been modified"
  },
  "user.notify.account_changed": {
    "fr": "votre compte a bien été modifié",
    "en": null
  }
}
```

This new format repeats several times the locales, but it presents the following advantages:
 - it becomes possible to search for a translation from the unique key
 - the missing translations are quicker to spot
 - as the translations for one key are gathered in one place, it is not necessary to browse several files to translate a value in every languages

## exercise

When the translation file is consequent (several thousands lines), and the number of languages is high, it can be painful to migrate without automation.

Would you be able to write a script to automate the work ? A test driven approach is advised !

## tips

This section is optional. It presents several technical functions that will help you trough the exercise.

<details>

* you can use `hash.dig('key1', 'key2', 'key3')` to dig quickly into a deep hash. [ref](https://ruby-doc.org/core-2.3.0_preview1/Hash.html#method-i-dig)
* the function `hash1.deep_merge(hash2)` will allow you to merge two deep hashes without overriding the former value [ref](https://apidock.com/rails/Hash/deep_merge). As this is a rails function, to have it work you must monkey-patch `Hash` with the code [here](https://github.com/casunlight/rails/blob/master/activesupport/lib/active_support/core_ext/hash/deep_merge.rb)
* the iterator `inject({})` will allow you to iterate over an array and inject the values you want in a resulting hash [ref](https://apidock.com/ruby/Enumerable/inject)
* the function `YAML.load_file('path/to/file')` will allow you to load the content of a yaml file and return it in a corresponding hash [ref](https://apidock.com/ruby/YAML/load_file/class)

</details>

## help

This section is optional. It will help you get through the exercise by giving you general advices and directions. :warning: It contains spoils regarding a technique to get trough. Unfurl at your own risks ! :warning:

<details>

General advices on the approach:
<details>
If you don't know how to begin, consider doing the exercise step by step:

* first create a small function capable of migrating a simple key, for a simple hash
* then you can create a function capable of migrating several nested keys, for a more complex hash
* then you can create a function capable of migrating a full file
* then you can create a function capable of migrating several files (for several languages)
</details>

Algorithms tips:
<details>
 You are stuck and you would like a tip on the algorithm to implement? A recursive strategy can help. Any other approach is welcome though
</details>
</details>

### Credits

* Thanks to [LaLibertad](https://github.com/lalibertad) for providing the translations [test set](https://github.com/lalibertad/consul/tree/master/config/locales)
* Thanls to [sunny](https://github.com/sunny) for the design of the flat translation format
