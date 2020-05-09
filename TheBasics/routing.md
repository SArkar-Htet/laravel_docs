# Routing
* [**Basic Routing**](#basicroute)
  * [Redirect Routes](#redirect-routes)
  * [View Routes](#view-routes)

* [**Route Parameters**](#route-parameters)
  * [Required Parameters](#required-parameters)
  * [Optional Parameters](#optional-parameters)
  * <a href="#regconstraints">Regular Expression Constraints</a>

* <a href="#parameter">**Named Routes**</a>

* <a href="#parameter">Route Groups</a>
  * <a href="#parameter">Middleware</a>
  * <a href="#parameter">Namespaces</a>
  * <a href="#parameter">Sub-Domain Routing</a>
  * <a href="#parameter">Route Prefixes</a>
  * <a href="#parameter">Route Name Prefixes</a>

* <a href="#parameter">**Route Model Binding**</a>
  * <a href="#parameter">Implicit Binding</a>
  * <a href="#parameter">Explicit Binding</a>

* <a href="#parameter">**Fallback Routes**</a>

* <a href="#parameter">**Rate Limiting**</a>

* <a href="#parameter">**Form Method Spoofing**</a>

* <a href="#parameter">**Accessing The Current Route**</a>

## <span id='basicroute'>Basic Routing</span>

### Routeဆိုတာ ဘာလဲ?
Routeဆိုတာ ကိုယ့်websiteရဲ့ ဘယ်လမ်းကြောင်းကိုသွားမယ် ဘယ်pageကိုသွားမယ်ဆိုတာ ညွှန်းပေးတဲ့အရာဖြစ်ပါတယ်
။
#### The Default Route Files
Route filesအားလုံးက `routes`ဆိုတဲ့ directoryထဲမှာ ရှိကြပါတယ်။ web interfaceအတွက် `routes/web.php` ဆိုတဲ့ fileထဲမှာ ရေးကြပါတယ်။ apiအတွက်တော့ `routes/api.php`ဆိုတဲ့ fileထဲမှာ ရေးကြရမှာပါ။ web applicationအများစုမှာ routeကို `routes/web.php`မှာ ရေးကြတာများပါတယ်။ 

`routes/api.php` fileထဲမှာရှိတဲ့ routeတွေသည် `/api` URI prefixကို အလိုအလျောက်ကြေငြာထားခဲ့တဲ့အတွက် သင့်အနေနဲ့ ဒီဖိုင်ထဲမှာရှိတဲ့ routeတိုင်းကို တစ်ဆင့်ချင်းကြေငြာနေစရာမလိုပါဘူး။ `RouteServiceProvider` classကို modifyလုပ်ချင်းအားဖြင့် သင့်အနေနဲ့ prefixနှင့် အခြားroute group option တွေကို modifyလုပ်နိုင်မှာ ဖြစ်ပါတယ်။

#### Available Router Methods
Routeတွေမှာအသုံးပြုတဲ HTTP verbတွေကို ဖော်ပြလိုက်ပါတယ်။

    Route::get($uri, $callback);
    Route::post($uri, $callback);
    Route::put($uri, $callback);
    Route::patch($uri, $callback);
    Route::delete($uri, $callback);
    Route::options($uri, $callback);

သင်ဟာ HTTP verb နှစ်ခုလောက်ကို respondလုပ်နိုင်တဲ့ routeတစ်ခုကို သတ်မှတ်ဖို့လိုလာတဲ့ အခါမှာ `match` methodကို သုံးနိုင်ပါတယ်။ ဒါမှမဟုတ် HTTP verbအားလုံးကို respondလုပ်လိုတဲ့ အခါဆိုရင်တော့ `any` methodကို သုံးနိုင်ပါတယ်။

    Route::match(['get', 'post'], '/', function () {
      //
    });

    Route::any('/', function () {
      //
    });

#### CSRF Protection
HTML formတိုင်းမှာ CSRF token field တစ်ခုပါဝင်သင့်ပါသည်။ မဟုတ်ရင် ဒီform requestက ပယ်ချချင်းခံရမှာပါ။ 

    <form method="POST" action="/profile">
        @csrf
        ...
    </form>

<a name="redirect-routes"></a>
### Redirect Routes
`Route::redirect` methodက redirectဆိုသည့်အတိုင်း ပထမparameterက URIကို ခေါ်လိုက်တာနဲ့ ဒုတိယparameterမှာရှိတဲ့ URIကို ရောက်ရှိသွားမှာ ဖြစ်ပါတယ်။ ဒီmethodသည် full routeတစ်ကြောင်းရေးပေးစရာမလိုသလို controller functon တစ်ခုလည်းရေးပေးစရာမလိုပါဘူး။

    Route::redirect('/here', '/there');

`Route::redirect`သည် ယေဘုယျအားဖြင့် `302` status codeတစ်ခုကို return ပြန်ပေးပါတယ်။ သင်အနေနဲ့ ဒီstatus codeကို third parameterထည့်ပြီး စိတ်ကြိုက်ပြင်ဆင်နိုင်ပါတယ်။

    Route::redirect('/here', '/there', 301);

301 status codeတစ်ခုကို returnပြန်ရန် `Route::permanentRedirect`ကိုလည်း အသုံးပြုနိုင်ပါတယ်။

    Route::permanentRedirect('/here', '/there');

<a name="view-routes"></a>
### View Routes

သင့်ရဲ့routeက viewကိုဘဲ returnပြန်ရန်လိုအပ်တယ်ဆိုပါစို့။ ဒါဆိုရင် `Route::view` methodကို သုံးနိုင်ပါတယ်။ ပထမargumentက URIဖြစ်ပြီး ဒုတိယargumentက view fileရဲ့ nameဖြစ်ပါတယ်။

    Route::view('/welcome', 'welcome');

 viewဆီကို dataထည့်ပေးလိုက်ဖို့ optional third argumentကို သုံးနိုင်ပါတယ်။

    Route::view('/welcome', 'welcome', ['name' => 'Taylor']);

<a name="route-parameters"></a>
## Route Parameters
<a name="required-parameters"></a>
### Required Parameters

တစ်ခါတစ်လေမှာ မင်းရဲ့routeထဲမှာ URIရဲ့ အစိတ်အပိုင်းတွေကို ဖမ်းယူရန်လိုလာလိမ့်မယ်။ ဥပမာ URIထဲက user idကို မင်းကဖမ်းဖို့လိုလာတယ်။ ဒါကို route parameterသတ်မှတ်ပေးပြီး လုပ်နိုင်ပါတယ်။

    Route::get('user/{id}', function ($id) {
        return 'User '.$id;
    });

အပေါ်က ဥပမာကိုကြည့်ပါ။ `{id}`ဆိုတာ route parameterပါ။ Browserရဲ့ URI barမှာ `user/1` ဒီလို မျိုးတွေ့ရရင် parameter `{id}`က `1` ကိုရည်ညွှန်းတာဖြစ်ပါတယ်။ function ထဲမှာရှိတဲ့ခ`$id` ဆိုတာက အဲဒီ route parameter `{id}` ကိုလှမ်းဖမ်းထားတာပါ။

မင်းရဲ့ routeမှာ လိုအပ်လာရင် route parameterတွေကို အများကြီသတ်မှတ်နိုင်ပါတယ်။

    Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
        //
    });

Route parametersတွေကို အမြဲတမ်း `{}` ထဲမှာ ပိတ်ထားပြီးတော့ alphabetic charactersတွေဘဲဖြစ်သင့်ပါတယ်။ `-`မသုံးရပါဘူး။ `-`အစား underscore( `_` )သုံးလို့ရပါတယ်။

<a name="required-parameters"></a>
### Optional Parameters

ဒီoptional parameterကတော့ optionalဆိုတဲ့အတိုင်း route parameterက optionalဖြစ်နေမှာပါ။ parameter nameနောက်မှာ `?` ထည့်ပေးပြီး optional parameterအဖြစ် သတ်မှတ်ရပါတယ်။ optional parameterသုံးပြီဆိုရင် သူ့ကို default valueပေးကို ပေးရပါတယ်။

    Route::get('user/{name?}', function ($name = null) {
        return $name;
    });

    Route::get('user/{name?}', function ($name = 'John') {
        return $name;
    });

`{name?}` ဆိုတာ optional parameterဖြစ်ပြီးတော့ functionထဲမှာ default valueပေးထားတာကို တွေ့နိုင်ပါတယ်။