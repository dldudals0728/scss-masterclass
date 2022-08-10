# Nomadcoders scss 작동 방식

scss는 gulpfile.babel.js 파일의 watch, src, dest로 작동한다.

```
watch: "src/scss/*",
src: "src/scss/styles.scss",
dest: "dist/css",
```

watch를 통해 명시된 경로의 파일들의 변화를 감지하고, src의 파일을 통해 dest 파일을 수정한다.

## HTML 파일의 link 태그는 반드시 css(not scss)를 가리킬 것!

```
<link rel="stylesheet" href="dist/css/styles.css" />
```

# 2. '\_'로 시작하는 .scss파일

'\_potato.scss'와 같은 파일은, css로 변환하지 않을 scss파일, 즉 scss만을 위한 파일로 사용된다.
이런 파일들은 css로 compile 되지 않는다.

# 3. scss의 변수 선언(in \_파일.scss)

변수명 앞에 $를 사용하고, 속성을 입력한다!

```
$bg: #e7473c;
```

그 후 scss파일에 사용하기 위해서는 import를 해주고 변수를 사용하면 된다.

### in styles.scss

```
@import "_파일.scss";
body { background: $bg; }
```

# 4. Nesting

코드를 더욱 읽기 쉽게!!

### before Nesting

```css
.box {
  margin-top: 20px;
}

.box h2 {
  color: blue;
}

.box button {
  color: red;
}
.box:hover {
  background-color: green;
}
```

### After Nesting

```scss
.box {
  margin-top: 20px;
  &:hover {
    background-color: green;
  }
  h2 {
    color: blue;
    &:hover {
      background-color: red;
    }
  }
  button {
    color: red;
  }
}
```

# 5. Mixins

css를 프로그래밍 언어의 함수처럼 사용할 수 있다.

### in \_mixins.scss

```scss
@mixin link($color) {
  text-decoration: none;
  display: block;
  color: $color;
}
```

### in styles.scss

```scss
@import "_mixins";
a {
  margin-bottom: 10px;
  &:nth-child(odd) {
    @include link(blue);
  }
  &:nth-child(even) {
    @include link(red);
  }
}
```

또는, if, else-if 와 같은 문법도 사용이 가능하다.

### in \_mixins.scss

```scss
@mixin link($word) {
  text-decoration: none;
  display: block;
  @if $word == "odd" {
    color: blue;
  } @else if $word == "even" {
    color: red;
  } @else {
    color: black;
  }
}
```

### in styles.scss

```scss
a {
  margin-bottom: 10px;
  &:nth-child(odd) {
    @include link("odd");
  }
  &:nth-child(even) {
    @include link("even");
  }
}
```

# 6. Extends

Extends: 같은 코드를 중복하고 싶지 않을 때 사용! 공통적인 부분을 만들어 코드에서 사용한다.

### in \_buttons.scss

```scss
%button {
  font-family: inherit;
  border-radius: 7px;
  font-size: 12px;
  text-transform: uppercase;
  padding: 5px 10px;
  background-color: peru;
  color: white;
  font-weight: 500;
}
```

### in styles.scss

```scss
@import "_buttons";

a {
  @extend %button;
  text-decoration: none;
}

button {
  @extend %button;
  border: none;
}
```
