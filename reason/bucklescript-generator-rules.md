# Using BuckleScript's built-in generator rules

In a BuckleScript project, there may be times you want to generate boilerplate
code of some sort during development. For example, if you're working on the
front-end, for example, and you use `tailwind-css`, you would have re-build your
project after changing the CSS file, which could be pretty frustrating.

To solve this, BuckleScript provides [built-in
support](https://bucklescript.github.io/docs/en/build-advanced#customize-rules-generators-support)
for generators, which allow you to avoid doing a `bsb -clean-world` whenever
your Tailwind changes. Instead, `bsb` watches for changes to your CSS in the
same way it watches for changes to your Reason files.

Here's the configuration you'd need to include in your `bsconfig.json` for the
previously mentioned Tailwind example:

```json
{
  "sources": [
    {
      "dir": "src",
      "subdirs": true,
      "generators": [
        {
          "name": "gen-tailwind",
          "edge": ["tailwind.css", ":", "styles.css"]
        }
      ]
    }
  ],
  "generators": [
    {
      "name": "gen-tailwind",
      "command": "tailwindcss build $in -o $out"
    }
  ]
}
```

> Note that `styles.css` is the base CSS file that you will be modifying, `tailwind.css` is the auto-generated CSS that your project should consume.
