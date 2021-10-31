# fast2s

A super-fast Chinese translation tool to translate Traditional Chinese to Simplified Chinese.

Use [fst](https://github.com/BurntSushi/fst) to build the translation state machine.

Usage:

```rust
let t = "企畫 計畫 企劃 計劃 畫圖 畫畫";
let s = fast2s::convert(k);
assert_eq!(&s, "企画 计画 企划 计划 画图 画画");
```

## Benchmark

See [simple.rs](benches/simple/benches/simple.rs) under benches directory. I compared the result with [opencc-rust](https://github.com/magiclen/opencc-rust), [simplet2s-rs](https://github.com/bosondata/simplet2s-rs), and [character_converter](https://github.com/sotch-pr35mac/character_converter). As character_converter is too slow, I have to change the sample size to 10 to not wait super long.

Test result (convert and return new string):

| tests | fast2s | simplet2s-rs | opencc-rust | character_conver |
| ----- | ------ | ------------ | ----------- | ---------------- |
| zht   | 596us  | 579us        | 4.93ms      | 1.23s            |
| zhc   | 643us  | 750us        | 5.89ms      | 2.87s            |
| en    | 59us   | 2.68ms       | 11.46ms     | 26.11s           |

Test result (mutate existing string):

| tests | fast2s | simplet2s-rs | opencc-rust | character_conver |
| ----- | ------ | ------------ | ----------- | ---------------- |
| zht   | 524us  | N/A          | N/A         | N/A              |
| zhc   | 609us  | N/A          | N/A         | N/A              |
| en    | 48us   | N/A          | N/A         | N/A              |

Note:

1. zht means load "math_zht.txt" and translate, zhc means load "math_zhc.txt" (all Simplified Chinese) and translate, en means load "math_en.txt" (all English) and translate.
2. N/A means not supported.

Please do not trust the benchmark result directly, you shall run it in your local environment. See [how to run benchmark](./benches/README.md).

## Credits

[t2s.txt](src/t2s.txt) is borrored from [simplet2s](https://github.com/bosondata/simplet2s-rs/blob/master/src/t2s.txt).
