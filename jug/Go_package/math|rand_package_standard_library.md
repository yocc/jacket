https://pkg.go.dev/math



### Overview [¶](https://pkg.go.dev/math#pkg-overview)

Package math provides basic constants and mathematical functions.

This package does not guarantee bit-identical results across architectures.

### Index [¶](https://pkg.go.dev/math#pkg-index)

- [Constants](https://pkg.go.dev/math#pkg-constants)
- [func Abs(x float64) float64](https://pkg.go.dev/math#Abs)
- [func Acos(x float64) float64](https://pkg.go.dev/math#Acos)
- [func Acosh(x float64) float64](https://pkg.go.dev/math#Acosh)
- [func Asin(x float64) float64](https://pkg.go.dev/math#Asin)
- [func Asinh(x float64) float64](https://pkg.go.dev/math#Asinh)
- [func Atan(x float64) float64](https://pkg.go.dev/math#Atan)
- [func Atan2(y, x float64) float64](https://pkg.go.dev/math#Atan2)
- [func Atanh(x float64) float64](https://pkg.go.dev/math#Atanh)
- [func Cbrt(x float64) float64](https://pkg.go.dev/math#Cbrt)
- [func Ceil(x float64) float64](https://pkg.go.dev/math#Ceil)
- [func Copysign(x, y float64) float64](https://pkg.go.dev/math#Copysign)
- [func Cos(x float64) float64](https://pkg.go.dev/math#Cos)
- [func Cosh(x float64) float64](https://pkg.go.dev/math#Cosh)
- [func Dim(x, y float64) float64](https://pkg.go.dev/math#Dim)
- [func Erf(x float64) float64](https://pkg.go.dev/math#Erf)
- [func Erfc(x float64) float64](https://pkg.go.dev/math#Erfc)
- [func Erfcinv(x float64) float64](https://pkg.go.dev/math#Erfcinv)
- [func Erfinv(x float64) float64](https://pkg.go.dev/math#Erfinv)
- [func Exp(x float64) float64](https://pkg.go.dev/math#Exp)
- [func Exp2(x float64) float64](https://pkg.go.dev/math#Exp2)
- [func Expm1(x float64) float64](https://pkg.go.dev/math#Expm1)
- [func FMA(x, y, z float64) float64](https://pkg.go.dev/math#FMA)
- [func Float32bits(f float32) uint32](https://pkg.go.dev/math#Float32bits)
- [func Float32frombits(b uint32) float32](https://pkg.go.dev/math#Float32frombits)
- [func Float64bits(f float64) uint64](https://pkg.go.dev/math#Float64bits)
- [func Float64frombits(b uint64) float64](https://pkg.go.dev/math#Float64frombits)
- [func Floor(x float64) float64](https://pkg.go.dev/math#Floor)
- [func Frexp(f float64) (frac float64, exp int)](https://pkg.go.dev/math#Frexp)
- [func Gamma(x float64) float64](https://pkg.go.dev/math#Gamma)
- [func Hypot(p, q float64) float64](https://pkg.go.dev/math#Hypot)
- [func Ilogb(x float64) int](https://pkg.go.dev/math#Ilogb)
- [func Inf(sign int) float64](https://pkg.go.dev/math#Inf)
- [func IsInf(f float64, sign int) bool](https://pkg.go.dev/math#IsInf)
- [func IsNaN(f float64) (is bool)](https://pkg.go.dev/math#IsNaN)
- [func J0(x float64) float64](https://pkg.go.dev/math#J0)
- [func J1(x float64) float64](https://pkg.go.dev/math#J1)
- [func Jn(n int, x float64) float64](https://pkg.go.dev/math#Jn)
- [func Ldexp(frac float64, exp int) float64](https://pkg.go.dev/math#Ldexp)
- [func Lgamma(x float64) (lgamma float64, sign int)](https://pkg.go.dev/math#Lgamma)
- [func Log(x float64) float64](https://pkg.go.dev/math#Log)
- [func Log10(x float64) float64](https://pkg.go.dev/math#Log10)
- [func Log1p(x float64) float64](https://pkg.go.dev/math#Log1p)
- [func Log2(x float64) float64](https://pkg.go.dev/math#Log2)
- [func Logb(x float64) float64](https://pkg.go.dev/math#Logb)
- [func Max(x, y float64) float64](https://pkg.go.dev/math#Max)
- [func Min(x, y float64) float64](https://pkg.go.dev/math#Min)
- [func Mod(x, y float64) float64](https://pkg.go.dev/math#Mod)
- [func Modf(f float64) (int float64, frac float64)](https://pkg.go.dev/math#Modf)
- [func NaN() float64](https://pkg.go.dev/math#NaN)
- [func Nextafter(x, y float64) (r float64)](https://pkg.go.dev/math#Nextafter)
- [func Nextafter32(x, y float32) (r float32)](https://pkg.go.dev/math#Nextafter32)
- [func Pow(x, y float64) float64](https://pkg.go.dev/math#Pow)
- [func Pow10(n int) float64](https://pkg.go.dev/math#Pow10)
- [func Remainder(x, y float64) float64](https://pkg.go.dev/math#Remainder)
- [func Round(x float64) float64](https://pkg.go.dev/math#Round)
- [func RoundToEven(x float64) float64](https://pkg.go.dev/math#RoundToEven)
- [func Signbit(x float64) bool](https://pkg.go.dev/math#Signbit)
- [func Sin(x float64) float64](https://pkg.go.dev/math#Sin)
- [func Sincos(x float64) (sin, cos float64)](https://pkg.go.dev/math#Sincos)
- [func Sinh(x float64) float64](https://pkg.go.dev/math#Sinh)
- [func Sqrt(x float64) float64](https://pkg.go.dev/math#Sqrt)
- [func Tan(x float64) float64](https://pkg.go.dev/math#Tan)
- [func Tanh(x float64) float64](https://pkg.go.dev/math#Tanh)
- [func Trunc(x float64) float64](https://pkg.go.dev/math#Trunc)
- [func Y0(x float64) float64](https://pkg.go.dev/math#Y0)
- [func Y1(x float64) float64](https://pkg.go.dev/math#Y1)
- [func Yn(n int, x float64) float64](https://pkg.go.dev/math#Yn)

#### Examples [¶](https://pkg.go.dev/math#pkg-examples)

- [Abs](https://pkg.go.dev/math#example-Abs)
- [Acos](https://pkg.go.dev/math#example-Acos)
- [Acosh](https://pkg.go.dev/math#example-Acosh)
- [Asin](https://pkg.go.dev/math#example-Asin)
- [Asinh](https://pkg.go.dev/math#example-Asinh)
- [Atan](https://pkg.go.dev/math#example-Atan)
- [Atan2](https://pkg.go.dev/math#example-Atan2)
- [Atanh](https://pkg.go.dev/math#example-Atanh)
- [Cbrt](https://pkg.go.dev/math#example-Cbrt)
- [Ceil](https://pkg.go.dev/math#example-Ceil)
- [Copysign](https://pkg.go.dev/math#example-Copysign)
- [Cos](https://pkg.go.dev/math#example-Cos)
- [Cosh](https://pkg.go.dev/math#example-Cosh)
- [Dim](https://pkg.go.dev/math#example-Dim)
- [Exp](https://pkg.go.dev/math#example-Exp)
- [Exp2](https://pkg.go.dev/math#example-Exp2)
- [Expm1](https://pkg.go.dev/math#example-Expm1)
- [Floor](https://pkg.go.dev/math#example-Floor)
- [Log](https://pkg.go.dev/math#example-Log)
- [Log10](https://pkg.go.dev/math#example-Log10)
- [Log2](https://pkg.go.dev/math#example-Log2)
- [Mod](https://pkg.go.dev/math#example-Mod)
- [Modf](https://pkg.go.dev/math#example-Modf)
- [Pow](https://pkg.go.dev/math#example-Pow)
- [Pow10](https://pkg.go.dev/math#example-Pow10)
- [Round](https://pkg.go.dev/math#example-Round)
- [RoundToEven](https://pkg.go.dev/math#example-RoundToEven)
- [Sin](https://pkg.go.dev/math#example-Sin)
- [Sincos](https://pkg.go.dev/math#example-Sincos)
- [Sinh](https://pkg.go.dev/math#example-Sinh)
- [Sqrt](https://pkg.go.dev/math#example-Sqrt)
- [Tan](https://pkg.go.dev/math#example-Tan)
- [Tanh](https://pkg.go.dev/math#example-Tanh)
- [Trunc](https://pkg.go.dev/math#example-Trunc)

### Constants [¶](https://pkg.go.dev/math#pkg-constants)

[View Source](https://cs.opensource.google/go/go/+/go1.16.7:src/math/const.go;l=11)

```
const (
	E   = 2.71828182845904523536028747135266249775724709369995957496696763 // https://oeis.org/A001113
	Pi  = 3.14159265358979323846264338327950288419716939937510582097494459 // https://oeis.org/A000796
	Phi = 1.61803398874989484820458683436563811772030917980576286213544862 // https://oeis.org/A001622

	Sqrt2   = 1.41421356237309504880168872420969807856967187537694807317667974 // https://oeis.org/A002193
	SqrtE   = 1.64872127070012814684865078781416357165377610071014801157507931 // https://oeis.org/A019774
	SqrtPi  = 1.77245385090551602729816748334114518279754945612238712821380779 // https://oeis.org/A002161
	SqrtPhi = 1.27201964951406896425242246173749149171560804184009624861664038 // https://oeis.org/A139339

	Ln2    = 0.693147180559945309417232121458176568075500134360255254120680009 // https://oeis.org/A002162
	Log2E  = 1 / Ln2
	Ln10   = 2.30258509299404568401799145468436420760110148862877297603332790 // https://oeis.org/A002392
	Log10E = 1 / Ln10
)
```

Mathematical constants.

[View Source](https://cs.opensource.google/go/go/+/go1.16.7:src/math/const.go;l=30)

```
const (
	MaxFloat32             = 3.40282346638528859811704183484516925440e+38  // 2**127 * (2**24 - 1) / 2**23
	SmallestNonzeroFloat32 = 1.401298464324817070923729583289916131280e-45 // 1 / 2**(127 - 1 + 23)

	MaxFloat64             = 1.797693134862315708145274237317043567981e+308 // 2**1023 * (2**53 - 1) / 2**52
	SmallestNonzeroFloat64 = 4.940656458412465441765687928682213723651e-324 // 1 / 2**(1023 - 1 + 52)
)
```

Floating-point limit values. Max is the largest finite value representable by the type. SmallestNonzero is the smallest positive, non-zero value representable by the type.

[View Source](https://cs.opensource.google/go/go/+/go1.16.7:src/math/const.go;l=39)

```
const (
	MaxInt8   = 1<<7 - 1
	MinInt8   = -1 << 7
	MaxInt16  = 1<<15 - 1
	MinInt16  = -1 << 15
	MaxInt32  = 1<<31 - 1
	MinInt32  = -1 << 31
	MaxInt64  = 1<<63 - 1
	MinInt64  = -1 << 63
	MaxUint8  = 1<<8 - 1
	MaxUint16 = 1<<16 - 1
	MaxUint32 = 1<<32 - 1
	MaxUint64 = 1<<64 - 1
)
```

Integer limit values.

### Variables [¶](https://pkg.go.dev/math#pkg-variables)

This section is empty.