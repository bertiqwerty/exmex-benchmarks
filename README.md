
# Exmex Benchmarks

[Exmex](https://github.com/bertiqwerty/exmex) is an extendable mathematical expression parser and evaluator. This project benchmarks exmex and compares to other crates that parse and evaluate mathematical expressions. 

Exmex was created with flexibility (e.g., use your own operators, literals, and types), ergonomics (e.g., just finds variables), and evaluation speed in mind. On the other hand, Exmex is slower than the other crates during parsing. However, evaluation might be more performance critical depending on the application. 

The expressions used to compare Exmex with other creates are:
```
sin:     "sin(x)+sin(y)+sin(z)",
power:   "x^2+y*y+z^z",
nested:  "x*0.02*sin(-(3*(2*sin(x-1/(sin(y*5)+(5.0-1/z))))))",
compile: "x*0.2*5/4+x*2*4*1*1*1*1*1*1*1+7*sin(y)-z/sin(3.0/2/(1-x*4*1*1*1*1))",
```
The following table shows mean runtimes of 5-evaluation-runs with increasing `x`-values on a Macbook Pro M1 Max in micro-seconds, i.e., smaller means better. [Criterion](https://docs.rs/criterion/latest/criterion/index.html)-based benchmarks can be executed via
```
cargo bench --all-features --bench benchmark -- --noplot --sample-size 10 --nresamples 10
```
to compute the results. Reported is the best result over multiple invocations. More about taking the minimum run-time for benchmarking can be found below.

## Latest Results

|                                                               | sin      | power    | nested   | compile  | comment                                        |
| ------------------------------------------------------------- | -------- | -------- | -------- | -------- | ---------------------------------------------- |
| [Evalexpr 12.0.2](https://docs.rs/evalexpr/12.0.2/evalexpr/) | 3.26     | 2.73     | 6.6      | 9.43     | more than mathematical expressions             |
| *[Exmex](https://docs.rs/exmex)* `f64`                        | **0.16** | **0.19** | **0.56** | **0.31** | can compute partial derivatives                |
| *[Exmex uncompiled](https://docs.rs/exmex)* `f64`             | **0.16** | **0.19** | **0.56** | 0.64     | can compute partial derivatives                |
| *[Exmex](https://docs.rs/exmex)* `Val`                        | 0.61     | 0.56     | 1.27     | 1.02     | multiple data types in one expression possible |


|                                                      | all expressions |
| ---------------------------------------------------- | --------------- |
| [Evalexpr](https://docs.rs/evalexpr/6.3.0/evalexpr/) | 21.92           |
| *[Exmex](https://docs.rs/exmex)* `f64`               | 36.69           |
| *[Exmex uncompiled](https://docs.rs/exmex)* `f64`    | 27.40           |
| *[Exmex](https://docs.rs/exmex)* `Val`               | 46.16           |

Exmex parsing can be made faster by passing only the relevant operators. 

Note that Criterion does [not provide the option to simply report the minimum runtime](https://bheisler.github.io/criterion.rs/book/analysis.html). A [talk by
Andrei Alexandrescu](https://youtu.be/vrfYLlR8X8k?t=1024) explains why I think taking the minimum is a good idea in many cases. See also https://github.com/bheisler/criterion.rs/issues/485.

Earlier versions of the benchmarks compared to [meval](https://docs.rs/meval/latest/meval/) and [fasteval](https://docs.rs/fasteval/latest/fasteval/). Both were measured to be significantly faster than Evalexpr. However, Exmex outperformed both of them in evaluation performance and was worse in parsing performance. However, Meval and Fasteval are not maintained anymore.
