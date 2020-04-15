# Routing
* [**Basic Routing**](#basicroute)
  * [Redirect Routes](#redirect-routes)
  * [View Routes](#view-routes)

* <a href="#parameters">**Route Parameters**</a>
  * <a href="#required">Required Parameters</a><br/>
  * <a href="#optional">Optional Parameters</a><br/>
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
Route filesအားလုံးသည် routesဆိုတဲ့ directoryထဲမှာ ရှိကြပါသည်။ web interfaceအတွက် routes/web.php ဆိုတဲ့ fileထဲမှာ ရေးကြပါသည်။ apiအတွက်တော့ routes/api.phpဆိုတဲ့ fileထဲမှာ ရေးကြရမှာပါ။ routeကို applicationအများစုမှာ routes/web.phpမှာ ရေးကြတာများပါတယ်။ 

routes/api.php fileထဲမှာရှိတဲ့ routeတွေသည် /api URI prefixကို အလိုအလျောက်ကြေငြာထားခဲ့တဲ့အတွက် သင့်အနေနဲ့ ဒီဖိုင်ထဲမှာရှိတဲ့ routeတိုင်းကို တစ်ဆင့်ချင်းကြေငြာနေစရာမလိုပါ။ RouteServiceProvider classကို modifyလုပ်ချင်းအားဖြင့် သင့်အနေနဲ့ prefixနှင့် အခြားroute group option တွေကို modifyလုပ်နိုင်မှာ ဖြစ်ပါတယ်။

#### Available Router Methods
The router allows you to register routes that respond to any HTTP verb:

    Route::get($uri, $callback);
    Route::post($uri, $callback);
    Route::put($uri, $callback);
    Route::patch($uri, $callback);
    Route::delete($uri, $callback);
    Route::options($uri, $callback);

သင်ဟာ HTTP verb နှစ်ခုလောက်ကို respondလုပ်နိုင်တဲ့ routeတစ်ခုကို သတ်မှတ်ရန်လိုလာတဲ့ အခါမှာ `match` methodကို သုံးနိုင်ပါတယ်။ ဒါမှမဟုတ် HTTP verbအားလုံးကို respondလုပ်လိုတဲ့ အခါဆိုရင်တော့ `any` methodကို သုံးနိုင်ပါတယ်။

    Route::match(['get', 'post'], '/', function () {
      //
    });

    Route::any('/', function () {
      //
    });

#### CSRF Protection
HTML formတိုင်းတွင် CSRF token field တစ်ခုပါဝင်သင့်ပါသည်။ မဟုတ်ရင် ဒီform requestက ပယ်ချချင်းခံရမှာပါ။ 

    <form method="POST" action="/profile">
        @csrf
        ...
    </form>

<a name="redirect-routes"></a>
### Redirect Routes
`Route::redirect` methodသည် redirectဆိုသည့်အတိုင်း ပထမparameterက URIကို ခေါ်လိုက်သည်နှင့် ဒုတိယparameterမှာရှိတဲ့ URIကို ရောက်ရှိသွားမှာ ဖြစ်ပါတယ်။ ဒီmethodသည် full routeတစ်ကြောင်းရေးပေးစရာမလိုသလို controller functon တစ်ခုလည်းရေးပေးစရာမလိုပါ။

    Route::redirect('/here', '/there');

`Route::redirect`သည် ယေဘုယျအားဖြင့် `302` status codeတစ်ခုကို return ပြန်ပေးပါသည်။ သင်သည် ဒီstatus codeကို third parameterထည့်ပြီး စိတ်ကြိုက်ပြင်ဆင်နိုင်ပါတယ်။

    Route::redirect('/here', '/there', 301);

301 status codeတစ်ခုကို returnပြန်ရန် `Route::permanentRedirect`ကိုလည်း အသုံးပြုနိုင်ပါတယ်။

    Route::permanentRedirect('/here', '/there');

<a name="view-routes"></a>
### View Routes

သင်ရဲ့routeသည် viewကိုဘဲ returnပြန်ရန်လိုအပ်သည်ဆိုလျှင် `Route::view` methodကို သုံးနိုင်ပါသည်။ ပထမargumentက URIဖြစ်ပြီး ဒုတိယargumentက view fileရဲ့ nameဖြစ်ပါသည်။

    Route::view('/welcome', 'welcome');

 ထို့အပြင် viewဆီသို့ optional third argumentအဖြစ်ဖြင့် dataပို့နိုင်ပါသည်။
 
    Route::view('/welcome', 'welcome', ['name' => 'Taylor']);
