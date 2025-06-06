<!--

@license Apache-2.0

Copyright (c) 2025 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# incrnanmstdev

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Compute a moving [corrected sample standard deviation][standard-deviation] incrementally, ignoring NaN values.

<section class="intro">

For a window of size `W`, the [corrected sample standard deviation][standard-deviation] is defined as

<!-- <equation class="equation" label="eq:corrected_sample_standard_deviation" align="center" raw="s = \sqrt{\frac{1}{W-1} \sum_{i=0}^{W-1} ( x_i - \bar{x} )^2}" alt="Equation for the corrected sample standard deviation."> -->

```math
s = \sqrt{\frac{1}{W-1} \sum_{i=0}^{W-1} ( x_i - \bar{x} )^2}
```

<!-- <div class="equation" align="center" data-raw-text="s = \sqrt{\frac{1}{W-1} \sum_{i=0}^{W-1} ( x_i - \bar{x} )^2}" data-equation="eq:corrected_sample_standard_deviation">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@49d8cabda84033d55d7b8069f19ee3dd8b8d1496/lib/node_modules/@stdlib/stats/incr/mstdev/docs/img/equation_corrected_sample_standard_deviation.svg" alt="Equation for the corrected sample standard deviation.">
    <br>
</div> -->

<!-- </equation> -->

</section>

<!-- /.intro -->



<section class="usage">

## Usage

```javascript
import incrnanmstdev from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmstdev@deno/mod.js';
```

#### incrnanmstdev( window\[, mean] )

Returns an accumulator `function` which incrementally computes a moving [corrected sample standard deviation][standard-deviation], ignoring `NaN` values. The `window` parameter defines the number of values over which to compute the moving [corrected sample standard deviation][standard-deviation].

```javascript
var accumulator = incrnanmstdev( 3 );
```

If the mean is already known, provide a `mean` argument.

```javascript
var accumulator = incrnanmstdev( 3, 5.0 );
```

#### accumulator( \[x] )

If provided an input value `x`, the accumulator function returns an updated [corrected sample standard deviation][standard-deviation]. If not provided an input value `x`, the accumulator function returns the current [corrected sample standard deviation][standard-deviation]. If provided `NaN`, the accumulator function ignores the value and returns the current [corrected sample standard deviation][standard-deviation] without updating the window.

```javascript
var accumulator = incrnanmstdev( 3 );

var s = accumulator();
// returns null

// Fill the window with non-NaN values...
s = accumulator( 2.0 ); // [2.0]
// returns 0.0

s = accumulator( NaN ); // [2.0]
// returns 0.0

s = accumulator( 3.0 ); // [2.0, 3.0]
// returns ~0.7071

s = accumulator( 5.0 ); // [2.0, 3.0, 5.0]
// returns ~1.53

// Window begins sliding...
s = accumulator( -7.0 ); // [3.0, 5.0, -7.0]
// returns ~6.43

s = accumulator( NaN ); // [3.0, 5.0, -7.0]
// returns ~6.43

s = accumulator( -5.0 ); // [5.0, -7.0, -5.0]
// returns ~6.43

s = accumulator();
// returns ~6.43
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   Input values are **not** type checked. If provided `NaN`, the value is ignored and the accumulator function returns the current [corrected sample standard deviation][standard-deviation] without updating the window. If non-numeric inputs are possible, you are advised to type check and handle accordingly **before** passing the value to the accumulator function.
-   As `W` values are needed to fill the window buffer, the first `W-1` returned values are calculated from smaller sample sizes. Until the window is full, each returned value is calculated from all provided values (excluding NaN values).

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
import randu from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-randu@deno/mod.js';
import incrnanmstdev from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-incr-nanmstdev@deno/mod.js';

var accumulator;
var v;
var i;

// Initialize an accumulator:
accumulator = incrnanmstdev( 5 );

// For each simulated datum, update the moving corrected sample standard deviation...
for ( i = 0; i < 100; i++ ) {
    if ( randu() < 0.2 ) {
        v = NaN;
    } else {
        v = randu() * 100.0;
    }
    accumulator( v );
}
console.log( accumulator() );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2025. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-incr-nanmstdev.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-incr-nanmstdev

[test-image]: https://github.com/stdlib-js/stats-incr-nanmstdev/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/stats-incr-nanmstdev/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-incr-nanmstdev/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-incr-nanmstdev?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-incr-nanmstdev.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-incr-nanmstdev/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/stats-incr-nanmstdev/tree/deno
[deno-readme]: https://github.com/stdlib-js/stats-incr-nanmstdev/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/stats-incr-nanmstdev/tree/umd
[umd-readme]: https://github.com/stdlib-js/stats-incr-nanmstdev/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/stats-incr-nanmstdev/tree/esm
[esm-readme]: https://github.com/stdlib-js/stats-incr-nanmstdev/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/stats-incr-nanmstdev/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-incr-nanmstdev/main/LICENSE

[standard-deviation]: https://en.wikipedia.org/wiki/Standard_deviation

<!-- <related-links> -->

<!-- </related-links> -->

</section>

<!-- /.links -->
