## پیوست ج: صفات مشتق پذیر

در جاهای مختلف کتاب، ویژگی `derive` را مورد بحث قرار داده‌ایم که می‌توانید آن را برای تعریف ساختار یا enum اعمال کنید. ویژگی «derive» کدی را تولید می‌کند که یک ویژگی را با اجرای پیش‌فرض خود روی نوعی که با نحو `derive` حاشیه‌نویسی کرده‌اید، پیاده‌سازی می‌کند.

در این پیوست، ما مرجعی از تمام صفات موجود در کتابخانه استاندارد ارائه می‌کنیم که می‌توانید با `اشتقاق` استفاده کنید. هر بخش شامل:

* چه عملگرها و روش هایی که این ویژگی را مشتق می کنند را قادر می سازد
* اجرای صفت ارائه شده توسط `اشتقاق` چه می کند
* اجرای صفت در مورد نوع دلالت دارد
* شرایطی که در آن شما مجاز به اجرای این صفت هستید یا نه
* نمونه هایی از عملیاتی که به این صفت نیاز دارند

اگر رفتاری متفاوت از رفتار ارائه شده توسط ویژگی `derive` می‌خواهید،
برای جزئیات نحوه پیاده‌سازی دستی هر صفت، به [مستندات کتابخانه استاندارد](../std/index.html)<!-- ignore --> مراجعه کنید.

بقیه صفات تعریف شده در کتابخانه استاندارد را نمی توان با استفاده از `derive` روی انواع شما پیاده سازی کرد.
این ویژگی‌ها رفتار پیش‌فرض معقولی ندارند،
بنابراین این شما هستید که باید آن‌ها را به گونه‌ای اجرا کنید که برای آنچه می‌خواهید انجام دهید منطقی باشد.

نمونه‌ای از ویژگی‌هایی که نمی‌توان آن را استخراج کرد `Display` است که قالب‌بندی را برای کاربران نهایی انجام می‌دهد.
همیشه باید روش مناسبی را برای نمایش یک نوع به کاربر نهایی در نظر بگیرید.
کاربر نهایی باید چه بخش هایی از نوع را ببیند؟ چه بخش هایی را مرتبط می دانند؟
چه فرمتی از داده ها می تواند بیشتر به آنها مربوط باشد؟
کامپایلر Rust این بینش را ندارد، بنابراین نمی تواند رفتار پیش فرض مناسبی را برای شما ارائه دهد.

فهرست صفات قابل مشتق ارائه شده در این پیوست جامع نیست:
کتابخانه‌ها می‌توانند `derive` را برای ویژگی‌های خود پیاده‌سازی کنند،
و فهرستی از ویژگی‌هایی را که می‌توانید از `derive` استفاده کنید، کاملاً باز ایجاد می‌کنند.
پیاده‌سازی `derive` شامل استفاده از یک ماکرو رویه‌ای است که در بخش ["Macros"][macros]<!-- ignore --> در فصل ۱۹ پوشش داده شده است.

###  `Debug` برای خروجی برنامه‌نویس

کتابخانه‌ی `Debug` قالب‌بندی اشکال‌زدایی را در رشته‌های قالبی فعال می‌کند،
که با افزودن «:؟» در جای‌بان‌های «{}» نشان می‌دهید.

کتابخانه‌ی `Debug` به شما امکان می‌دهد نمونه‌هایی از یک نوع را برای اهداف اشکال‌زدایی چاپ کنید،
بنابراین شما و سایر برنامه‌نویسانی که از نوع داده‌ی شما استفاده می‌کنند،
می‌توانید یک نمونه را در نقطه خاصی از اجرای برنامه بررسی کنید.

برای مثال در استفاده از ماکرو «assert_eq!» به کتابخانه‌ی `Debug`  نیاز است.
اگر ادعای برابری ناموفق باشد، این ماکرو مقادیر نمونه‌هایی را که به‌عنوان آرگومان ارائه می‌شوند،
چاپ می‌کند تا برنامه‌نویسان بتوانند ببینند که چرا این دو نمونه برابر نبودند.

### 'PartialEq` و `Eq` برای مقایسه برابری

ویژگی `PartialEq` به شما امکان می دهد نمونه هایی از یک نوع‌داده را برای بررسی برابری مقایسه کنید
و استفاده از عملگرهای `==` و `!=` را فعال می کند.

استخراج `PartialEq` روش `eq` را پیاده‌سازی می‌کند. چه زمانی
PartialEq` بر روی ساختارها مشتق می شود،
دو نمونه تنها در صورتی برابر هستند که *همه*‌ی فیلدها برابر باشند، 
و نمونه ها برابر نیستند اگر هیچ‌یک از فیلدها مساوی نباشند.
هنگامی که بر روی enums مشتق می شود، هر گونه‌داده با خود برابر است و با سایر انواع‌داده برابر نیست.

برای مثال، با استفاده از ماکرو `assert_eq!` که باید بتواند 
دو حالت از یک نوع‌داده را برای برابری مقایسه کند، ویژگی `PartialEq` مورد نیاز است.

صفت `Eq` هیچ روشی ندارد. هدف آن نشان دادن این است که برای هر مقدار از نوع‌داده‌ی مشروح، مقدار با خودش برابر است.
ویژگی `Eq` را فقط می‌توان برای انواعی اعمال کرد که `PartialEq` را نیز پیاده‌سازی می‌کنند،
اگرچه همه‌ی انواعی که `PartialEq` را پیاده‌سازی می‌کنند نمی‌توانند `Eq` را پیاده‌سازی کنند. 
یکی از نمونه‌های آن انواع‌داده‌ی اعداد ممیز شناور است: 
اجرای اعداد ممیز شناور بیان می‌کند که دو نمونه از مقدار غیر عددی (NaN) با یکدیگر برابر نیستند.

یک مثال از زمانی که `Eq` مورد نیاز است، برای کلیدهای `HashMap<K, V>` است، بنابراین
`HashMap<K, V>` می تواند تشخیص دهد که آیا دو کلید یکسان هستند یا خیر.

### `PartialOrd` و `Ord` برای مقایسه‌های سفارش

ویژگی «PartialOrd» به شما امکان می‌دهد نمونه‌هایی از یک نوع را برای اهداف مرتب‌سازی مقایسه کنید.
نوعی که «PartialOrd» را پیاده‌سازی می‌کند،
می‌تواند با عملگرهای «<»، «>»، «<=» و «>=» استفاده شود.
شما فقط می‌توانید ویژگی «PartialOrd» را برای انواعی اعمال کنید که «PartialEq» را نیز پیاده‌سازی می‌کنند.

ارث‌بری‌کننده از  `PartialOrd` متد `partial_cmp` را پیاده‌سازی می‌کند، که یک
`Option<Ordering>`
 را برمی‌گرداند که وقتی مقادیر داده‌شده ترتیبی ایجاد نمی‌کنند، `None` خواهد بود.
نمونه‌ای از مقداری که ترتیبی ایجاد نمی‌کند،
حتی اگر بیشتر مقادیر از این نوع را می‌توان مقایسه کرد، مقدار ممیز شناور غیر عددی (NaN) است. فراخوانی `partial_cmp` با هر عدد ممیز شناور
و مقدار ممیز شناور `NaN` `None` را برمی‌گرداند.

When derived on structs, `PartialOrd` compares two instances by comparing the
value in each field in the order in which the fields appear in the struct
definition. When derived on enums, variants of the enum declared earlier in the
enum definition are considered less than the variants listed later.

The `PartialOrd` trait is required, for example, for the `gen_range` method
from the `rand` crate that generates a random value in the range specified by a
low value and a high value.

The `Ord` trait allows you to know that for any two values of the annotated
type, a valid ordering will exist. The `Ord` trait implements the `cmp` method,
which returns an `Ordering` rather than an `Option<Ordering>` because a valid
ordering will always be possible. You can only apply the `Ord` trait to types
that also implement `PartialOrd` and `Eq` (and `Eq` requires `PartialEq`). When
derived on structs and enums, `cmp` behaves the same way as the derived
implementation for `partial_cmp` does with `PartialOrd`.

An example of when `Ord` is required is when storing values in a `BTreeSet<T>`,
a data structure that stores data based on the sort order of the values.

### `Clone` and `Copy` for Duplicating Values

The `Clone` trait allows you to explicitly create a deep copy of a value, and
the duplication process might involve running arbitrary code and copying heap
data. See the [“Ways Variables and Data Interact:
Clone”][ways-variables-and-data-interact-clone]<!-- ignore --> section in
Chapter 4 for more information on `Clone`.

Deriving `Clone` implements the `clone` method, which when implemented for the
whole type, calls `clone` on each of the parts of the type. This means all the
fields or values in the type must also implement `Clone` to derive `Clone`.

An example of when `Clone` is required is when calling the `to_vec` method on a
slice. The slice doesn’t own the type instances it contains, but the vector
returned from `to_vec` will need to own its instances, so `to_vec` calls
`clone` on each item. Thus, the type stored in the slice must implement `Clone`.

The `Copy` trait allows you to duplicate a value by only copying bits stored on
the stack; no arbitrary code is necessary. See the [“Stack-Only Data:
Copy”][stack-only-data-copy]<!-- ignore --> section in Chapter 4 for more
information on `Copy`.

The `Copy` trait doesn’t define any methods to prevent programmers from
overloading those methods and violating the assumption that no arbitrary code
is being run. That way, all programmers can assume that copying a value will be
very fast.

You can derive `Copy` on any type whose parts all implement `Copy`. You can
only apply the `Copy` trait to types that also implement `Clone`, because a
type that implements `Copy` has a trivial implementation of `Clone` that
performs the same task as `Copy`.

The `Copy` trait is rarely required; types that implement `Copy` have
optimizations available, meaning you don’t have to call `clone`, which makes
the code more concise.

Everything possible with `Copy` you can also accomplish with `Clone`, but the
code might be slower or have to use `clone` in places.

### `Hash` for Mapping a Value to a Value of Fixed Size

The `Hash` trait allows you to take an instance of a type of arbitrary size and
map that instance to a value of fixed size using a hash function. Deriving
`Hash` implements the `hash` method. The derived implementation of the `hash`
method combines the result of calling `hash` on each of the parts of the type,
meaning all fields or values must also implement `Hash` to derive `Hash`.

An example of when `Hash` is required is in storing keys in a `HashMap<K, V>`
to store data efficiently.

### `Default` for Default Values

The `Default` trait allows you to create a default value for a type. Deriving
`Default` implements the `default` function. The derived implementation of the
`default` function calls the `default` function on each part of the type,
meaning all fields or values in the type must also implement `Default` to
derive `Default`.

The `Default::default` function is commonly used in combination with the struct
update syntax discussed in the [“Creating Instances From Other Instances With
Struct Update
Syntax”][creating-instances-from-other-instances-with-struct-update-syntax]<!-- ignore -->
section in Chapter 5. You can customize a few fields of a struct and then
set and use a default value for the rest of the fields by using
`..Default::default()`.

The `Default` trait is required when you use the method `unwrap_or_default` on
`Option<T>` instances, for example. If the `Option<T>` is `None`, the method
`unwrap_or_default` will return the result of `Default::default` for the type
`T` stored in the `Option<T>`.

[creating-instances-from-other-instances-with-struct-update-syntax]:
ch05-01-defining-structs.html#creating-instances-from-other-instances-with-struct-update-syntax
[stack-only-data-copy]:
ch04-01-what-is-ownership.html#stack-only-data-copy
[ways-variables-and-data-interact-clone]:
ch04-01-what-is-ownership.html#ways-variables-and-data-interact-clone
[macros]: ch19-06-macros.html#macros
