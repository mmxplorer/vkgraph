BEGIN_XML
<graph>
<nodes k="r" l="env" csv_pipe>
i|ci
repo:VKUI|true
</nodes>

<nodes k="st" l="env" csv_pipe>
i
site:vkui.io
</nodes>

<nodes k="p" l="providers" idPrefix="pkg:@vkontakte/" csv_pipe>
i|v
vkui|7.4.0
vkui-floating-ui|0.2.4
vkui-date-fns-tz|0.0.4
</nodes>

<nodes k="p" l="tooling" idPrefix="pkg:@vkontakte/" csv_pipe>
i|v
vkui-codemods|1.0.0
</nodes>

<nodes k="p" l="docs" idPrefix="pkg:@vkontakte/" csv_pipe>
i|v
vkui-docs-theme|0.0.1
</nodes>

<nodes k="x" l="providers" idPrefix="ext:" csv_pipe>
i
react
react-dom
@swc/helpers
@vkontakte/icons
@vkontakte/vkjs
date-fns
@floating-ui/react-dom
@vkontakte/vkui-tokens
</nodes>

<nodes k="x" l="tooling" idPrefix="ext:" csv_pipe>
i
chalk
commander
cross-spawn
jscodeshift
prompts
typescript
@swc/core
@swc/jest
@types/jest
jest
@date-fns/tz
</nodes>

<nodes k="x" l="docs" idPrefix="ext:" csv_pipe>
i
next
nextra
</nodes>

<nodes k="tok" l="providers" csv_pipe>
i
tokens:vkui
</nodes>

<nodes k="as" l="providers" csv_pipe>
i|path
asset:vkui_css|@vkontakte/vkui/dist/vkui.css
</nodes>

<nodes k="po" l="policies" idPrefix="pol:" csv_pipe>
i
code_quality
theming
a11y
</nodes>

<nodes k="ln" l="policies" idPrefix="lint:eslint:" csv_pipe>
i
react-compiler
import/order
curly
eqeqeq
no-console
</nodes>

<nodes k="ln" l="policies" idPrefix="lint:stylelint:" csv_pipe>
i
prettier/prettier
vkui-internal/bad-multiplication
plugin/logical-shorthands
csstools/use-logical
</nodes>

<nodes k="ci" l="observability" idPrefix="ci:" csv_pipe>
i
eslint
stylelint
typecheck
test
</nodes>

<edges t="dp" s="pkg:@vkontakte/vkui-date-fns-tz" sc="dev" destPrefix="ext:" csv_pipe>
i
@date-fns/tz
@swc/core
typescript
</edges>

<edges t="ct" s="repo:VKUI" destPrefix="pkg:@vkontakte/" csv_pipe>
i
vkui
vkui-floating-ui
vkui-date-fns-tz
vkui-codemods
vkui-docs-theme
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui" sc="peer" destPrefix="ext:" csv_pipe>
i
react
react-dom
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui" sc="runtime" destPrefix="ext:" csv_pipe>
i
@swc/helpers
@vkontakte/icons
@vkontakte/vkjs
date-fns
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui" sc="runtime" destPrefix="pkg:@vkontakte/" csv_pipe>
i
vkui-date-fns-tz
vkui-floating-ui
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui-floating-ui" sc="peer" destPrefix="ext:" csv_pipe>
i
react
react-dom
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui-floating-ui" sc="runtime" destPrefix="ext:" csv_pipe>
i
@floating-ui/react-dom
@swc/helpers
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui-codemods" sc="runtime" destPrefix="ext:" csv_pipe>
i
chalk
commander
cross-spawn
jscodeshift
prompts
typescript
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui-docs-theme" sc="runtime" destPrefix="ext:" csv_pipe>
i
@vkontakte/icons
@vkontakte/vkjs
@vkontakte/vkui-tokens
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui-docs-theme" sc="runtime" destPrefix="pkg:@vkontakte/" csv_pipe>
i
vkui
</edges>

<edges t="dp" s="pkg:@vkontakte/vkui-docs-theme" sc="peer" destPrefix="ext:" csv_pipe>
i
next
nextra
react
react-dom
</edges>

<edges t="cb" s="repo:VKUI" destPrefix="ci:" csv_pipe>
i
eslint
stylelint
typecheck
test
</edges>

<edges t="ef" s="repo:VKUI" destPrefix="pol:" csv_pipe>
i
code_quality
theming
a11y
</edges>

<edges t="ct" s="pol:code_quality" destPrefix="lint:eslint:" csv_pipe>
i
react-compiler
import/order
curly
eqeqeq
no-console
</edges>

<edges t="ct" s="pol:theming" destPrefix="lint:stylelint:" csv_pipe>
i
prettier/prettier
vkui-internal/bad-multiplication
plugin/logical-shorthands
csstools/use-logical
</edges>

<edges t="ct" s="pol:theming" csv_pipe>
i
asset:vkui_css
</edges>

<edges t="cw" s="pkg:@vkontakte/vkui" destPrefix="pol:" csv_pipe>
i
code_quality
theming
a11y
</edges>

<edges t="ut" s="pkg:@vkontakte/vkui" csv_pipe>
i
tokens:vkui
</edges>

<edges t="rq" s="pkg:@vkontakte/vkui" csv_pipe>
i
asset:vkui_css
</edges>
</graph>
END_XML