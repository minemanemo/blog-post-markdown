# π€ μ¦μ

![screensh](./error_msg_1.png)

- svg νμΌμ λ‘λνλ μ»΄ν¬λνΈλ₯Ό Storybookμμ λΆλ¬μμΌλ μμ κ°μ λ©μΈμ§κ° λ°μνμλ€.
- λ°μμ  λΆμ λ°λ‘ π μ‘°μΉ μ¬ν­ νμΈ

# π§ μμΈ

## 1. svg νμΌ λ‘λλ₯Ό μν ν¨ν€μ§ μ€μΉ

> webpackμμ svg νμΌμ λ‘λνκΈ° μν΄μ μλ ν¨ν€μ§λ₯Ό μ€μΉνμλ€.

```bash
npm install -D @svgr/webpack
# λλ
yarn add -D @svgr/webpack
```

## 2. κΈ°μ‘΄μ storybookμ main.jsμ webpack μ€μ  μμ€ μ½λ

> svg νμΌμ λ‘λνκΈ° μν΄μλ webpack μ€μ μ΄ μ ν λμ΄μΌνκ³  λ³ΈμΈμ μλμ κ°μ΄ μ€μ νμμ.

```javascript
module.exports = {
  webpackFinal: async (config) => {
    config.module.rules.unshift({
      test: /\.svg$/,
      use: ['@svgr/webpack'],
    });
    return config;
  },
};
```

- κΈ°μ‘΄μ ruleμμ svg νμ₯μλ‘ λλλ νμΌμ λνμ¬ `@svgr/webpack`μ ν΅νμ¬ λ‘λνλλ‘ μ€μ ν κ²μ΄λ€.
- JavaScriptμ `unshift`λ₯Ό μ¬μ©νλ©΄ Arrayμ μ μΌ μ²μμ μ½μλ¨
- Webpackμ Ruleμ΄ μμ°¨μ μΌλ‘ μ μ©λλ κ²μΌλ‘ μκ°νκ³  μμ κ°μ΄ μ€μ  (μ¬μ€ μμΉ­μ ν΅ν΄μ λ³΅λΆν κ²;;γ)

## 3. μμΈμ μ°ΎκΈ° μν΄ main.jsλ₯Ό μλμ κ°μ΄ μμ 

```javascript
module.exports = {
  webpackFinal: async (config) => {
    console.log(config.module.rules);
    config.module.rules.unshift({
      test: /\.svg$/,
      use: ['@svgr/webpack'],
    });
    return config;
  },
};
```

- webpack ruleμ νμΈνκΈ° μνμ¬ `console.log`λ₯Ό μ°μ νλ©΄

## 4. storybook μ€ν ν μλμ κ°μ λ‘κ·Έ λ°μ

![screensh](./log_msg_1.png)

- webpack ruleμ νμΈν΄λ³΄λ μμ κ°μ΄ svg νμ₯μμ λν λ€λ₯Έ λ£°μ΄ μ μ©λλ κ²μ νμΈνμμ

## 5. μμλ‘ μ‘°μΉνμ¬ μ μ λμ νμΈ

```javascript
module.exports = {
  webpackFinal: async (config) => {
    console.log(config.module.rules);
    config.module.rules[config.module.rules.length - 2].test =
      /\.(ico|jpg|jpeg|png|apng|gif|eot|otf|webp|ttf|woff|woff2|cur|ani|pdf)(\?.*)?$/;
    config.module.rules.unshift({
      test: /\.svg$/,
      use: ['@svgr/webpack'],
    });
    return config;
  },
};
```

- μμ κ°μ΄ file-loaderμμ νμΌμ κ²μ¬νλ test regexμμ svg νμ₯μλ§ μ μΈνμλλ svg λ‘λ©μ΄ λλ κ²μ νμΈνλ€.
- arrayμ μμλ μ κ° μμΈ νμμ μν΄μ μμλ‘ μ¬μ©νμμ΅λλ€. (μ€μ μ λ°λΌμ ruleμ μμκ° λ€λ₯Ό μ μμ΅λλ€.)

# π μ‘°μΉ

```javascript
module.exports = {
  webpackFinal: async (config) => {
    const rules = config.module.rules;
    const fileLoaderRule = rules.find((rule) => rule.test.test('.svg'));
    fileLoaderRule.exclude = /\.svg$/;

    rules.push({
      test: /\.svg$/,
      use: ['@svgr/webpack'],
    });

    return config;
  },
};
```

- μμ κ°μ΄ svg νμ₯μλ‘ κ²μ¬νλ ruleμ μ°Ύμ exclude μ΅μμ ν΅νμ¬ κ²μ¬μμ μ μΈνλλ‘ νμμ΅λλ€.
- μμ μμ€μ½λλ svg κ²μ¬νλ rule νλλ§ μ°Ύμμ exclude μν€μ§λ§ μ¬λ¬κ°μ ruleμΌ μ μμΌλ λμ λ°λΌμ μ¬λ¬λ² μ°Ύμμ exclude μμΌμ€μΌ ν  μ μμ΅λλ€.
