# VK API Schema Typescript Generator

[![npm version](https://badge.fury.io/js/%40vkontakte%2Fapi-schema-typescript-generator.svg)](https://www.npmjs.com/package/@vkontakte/api-schema-typescript-generator)

This package generates TypeScript typings based on the [VK API JSON schema](https://github.com/VKCOM/vk-api-schema).

## Usage

#### Via npx:

```shell script
npx @vkontakte/api-schema-typescript-generator --schemaDir ./schema --outDir ./types/api --methods '*'
```

#### Via installed dependency:

```shell script
yarn add @vkontakte/api-schema-typescript-generator

vk-api-schema-typescript-generator --schemaDir ./schema --outDir ./types/api --methods 'messages.*'
```

### Options

#### --help

Shows help. 

#### --schemaDir

The relative path to directory with `methods.json`, `objects.json` and `responses.json` files.

#### --outDir

The directory where the files will be generated.

If you skip this param, script will work in linter mode without emitting files to file system.

> **Please note** that this folder will be cleared after starting the generation.

#### --methods

List of methods to generate responses and all needed objects.

Examples:

- `'*'` – to generate all responses and objects.
- `'messages.*, users.get, groups.isMember'`  - to generate all methods from messages section, users.get and groups.isMember.
