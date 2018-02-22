## Laravel Debugbar
[![Packagist License](https://poser.pugx.org/barryvdh/laravel-debugbar/license.png)](http://choosealicense.com/licenses/mit/)
[![Latest Stable Version](https://poser.pugx.org/barryvdh/laravel-debugbar/version.png)](https://packagist.org/packages/barryvdh/laravel-debugbar)
[![Total Downloads](https://poser.pugx.org/barryvdh/laravel-debugbar/d/total.png)](https://packagist.org/packages/barryvdh/laravel-debugbar)

### Ghi chú cho phiên bản 3:Debugbar hiện tại bạn đã có thể require (yêu cầu) gói Debugbar nhưng vẫn cần để APP_DEBUG = true như mặc định!!

### Đối với Laravel < 5.5, Hãy sử dụng nhánh [2.4](https://github.com/barryvdh/laravel-debugbar/tree/2.4)!

Đây là gói tích hợp [PHP Debug Bar](http://phpdebugbar.com/) với Laravel 5.
Nó bao gồm ServiceProvider để đăng ký debugbar và gắn nó vào đầu ra. Bạn có thể xuất bản assets và cấu hình nó trên Laravel.
Nó hỗ trợ 1 vài Collector để làm việc với Laravel và thực hiện 1 vài tùy chỉnh DataCollectors,cụ thẻ với Laravel.
Nó được cấu hình để hiển thị Redirects và (jQuery) Ajax Requests. (Trình bày trong danh sách thả xuống) Đọc tài liệu để biết thêm các tùy chọn cấu hình.
Read [the documentation](http://phpdebugbar.com/docs/) for more configuration options.

![Screenshot](https://cloud.githubusercontent.com/assets/973269/4270452/740c8c8c-3ccb-11e4-8d9a-5a9e64f19351.png)

Lưu ý: Chỉ sử dụng DebugBar trong quá trình phát triển. Nó có thể làm chậm ứng dụng xuống (vì nó đã thu thập dữ liệu). Vì vậy, khi gặp tình trạng chậm chạp, hãy thử vô hiệu hóa một số người thu gom.

Gói này bao gồm một số nhà sưu tập tùy chỉnh:
 - QueryCollector: Hiển thị tất cả các truy vấn, bao gồm cả ràng buộc + thời gian
 - RouteCollector: Hiển thị thông tin về Route hiện tại.
 - ViewCollector:  Hiển thị chế độ xem đang được tải. (Tùy chọn: hiển thị dữ liệu được chia sẻ)
 - EventsCollector: Hiển thị tất cả sự kiện
 - LaravelCollector: Hiển thị phiên bản Laravel và Môi trường. (tắt theo mặc định)
 - SymfonyRequestCollector: thay thế RequestCollector với thêm thông tin về yêu cầu / phản hồi
 - LogsCollector: Hiển thị các mục nhật ký mới nhất từ ​​các bản ghi lưu trữ. (tắt theo mặc định)
 - FilesCollector: Hiển thị các tệp được bao gồm / yêu cầu bởi PHP. (tắt theo mặc định)
 - ConfigCollector: Hiển thị các giá trị từ các tệp tin cấu hình. (tắt theo mặc định)
 - CacheCollector:Hiển thị tất cả sự kiện bộ nhớ cache. (tắt theo mặc định)

Nạp những collectors theo Laravel:
 - LogCollector: Hiển thị tất cả Log Messages
 - SwiftMailCollector and SwiftLogCollector for Mail

Và collectors mặc định:
 - PhpInfoCollector
 - MessagesCollector
 - TimeDataCollector (With Booting and Application timing)
 - MemoryCollector
 - ExceptionsCollector

Nó cũng cung cấp một giao diện Facade để dễ dàng ghi lại các thông báo, ngoại lệ và thời gian

## Cài đặt

Yêu cầu gói này với composer. Chỉ nên yêu cầu gói hàng để phát triển.

```shell
composer require barryvdh/laravel-debugbar --dev
```

Laravel 5.5 sử dụng gói Auto-Discovery,do đó, không yêu cầu bạn phải tự thêm ServiceProvider.

Debugbar sẽ được kích hoạt khi `APP_DEBUG` là `true`.

>Nếu bạn sử dụng một Route đường bắt / tất cả, hãy chắc chắn rằng bạn tải các ServiceProvider Debugbar trước các ServiceProviders App của riêng bạn.

### Laravel 5.5+:

Nếu bạn không sử dụng auto-discovery, thêm ServiceProvider vào mảng providers in config/app.php

```php
Barryvdh\Debugbar\ServiceProvider::class,
```

Nếu bạn muốn sử dụng facade để log messages, thêm nó vào facades của bạn in app.php:

```php
'Debugbar' => Barryvdh\Debugbar\Facade::class,
```

Trình hồ sơ được kích hoạt theo mặc định, nếu bạn có APP_DEBUG = true. Bạn có thể ghi đè điều đó trong cấu hình (debugbar.enabled) hoặc bằng cách đặt DEBUGBAR_ENABLED trong .vv của bạn. Xem thêm các tùy chọn trong config / debugbar.php Bạn cũng có thể đặt trong cấu hình của bạn nếu bạn muốn bao gồm / loại trừ các tập tin nhà cung cấp cũng (FontAwesome, Highlight.js và jQuery). Nếu bạn đã sử dụng chúng trong trang web của bạn, đặt nó là sai. Bạn cũng có thể chỉ hiển thị các nhà cung cấp js hoặc css, bằng cách đặt nó là 'js' hoặc 'css'. (Highlight.js yêu cầu cả css + js, vì vậy thiết lập để đúng cho cú pháp tô sáng)

Sao chép cấu hình gói sang cấu hình cục bộ của bạn bằng lệnh publish:

```shell
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
```

### Lumen:

Đối với Lumen, đăng ký 1 Provider khác trong `bootstrap/app.php`:

```php
if (env('APP_DEBUG')) {
 $app->register(Barryvdh\Debugbar\LumenServiceProvider::class);
}
```

Để thay đổi configuration, sao chép tập tin vào thư mục config của bạn và kích hoạt nó:

```php
$app->configure('debugbar');
```

## Cách sử dụng

Bây giờ bạn có thể thêm các tin nhắn bằng cách sử dụng Facade (khi bổ sung), sử dụng mức PSR-3 (gỡ lỗi, thông tin, thông báo, cảnh báo, lỗi, critical, alert, emergency):

```php
Debugbar::info($object);
Debugbar::error('Error!');
Debugbar::warning('Watch out…');
Debugbar::addMessage('Another message', 'mylabel');
```

VÀ start/stop thời gian:

```php
Debugbar::startMeasure('render','Time for rendering');
Debugbar::stopMeasure('render');
Debugbar::addMeasure('now', LARAVEL_START, microtime(true));
Debugbar::measure('My long operation', function() {
    // Do something…
});
```

Hoặc log ngoại lệ:

```php
try {
    throw new Exception('foobar');
} catch (Exception $e) {
    Debugbar::addThrowable($e);
}
```

Ngoài ra còn có các chức năng trợ giúp cho những cuộc gọi phổ biến nhất:

```php
// All arguments will be dumped as a debug message
debug($var1, $someString, $intValue, $object);

start_measure('render','Time for rendering');
stop_measure('render');
add_measure('now', LARAVEL_START, microtime(true));
measure('My long operation', function() {
    // Do something…
});
```

Nếu bạn muốn bạn có thể thêm DataCollectors của riêng bạn, thông qua Container hoặc Facade:

```php
Debugbar::addCollector(new DebugBar\DataCollector\MessagesCollector('my_messages'));
//Or via the App container:
$debugbar = App::make('debugbar');
$debugbar->addCollector(new DebugBar\DataCollector\MessagesCollector('my_messages'));
```

Theo mặc định, thanh Debugbar được tiêm ngay trước </ body>. Nếu bạn muốn tự mình chèn Debugbar, hãy đặt tùy chọn cấu hình 'inject' thành false và sử dụng trình kết xuất chính mình và làm theo http://phpdebugbar.com/docs/rendering.html

```php
$renderer = Debugbar::getJavascriptRenderer();
```

Lưu ý: Không sử dụng tự động chèn, sẽ vô hiệu hóa thông tin Yêu cầu, bởi vì nó được thêm vào Sau phản hồi. Bạn có thể thêm default_request datacollector trong config dưới dạng thay thế.

## Kích hoạt / Tắt trên thời gian chạy
Bạn có thể bật hoặc tắt thanh gỡ lỗi trong thời gian chạy.

```php
\Debugbar::enable();
\Debugbar::disable();
```

NB. Sau khi được kích hoạt, người sưu tập được thêm vào (và có thể tạo thêm chi phí), vì vậy nếu bạn muốn sử dụng debugbar trong sản xuất, vô hiệu hóa trong cấu hình và chỉ kích hoạt khi cần thiết.


## Tích hợp Twig

Laravel Debugbar đi kèm với hai Tiện ích mở rộng Twig. Đây là những thử nghiệm với rcrowe / TwigBridge 0.6.x

Thêm các phần mở rộng sau vào TwigBridge config / extensions.php của bạn (hoặc đăng ký các phần mở rộng theo cách thủ công)

```php
'Barryvdh\Debugbar\Twig\Extension\Debug',
'Barryvdh\Debugbar\Twig\Extension\Dump',
'Barryvdh\Debugbar\Twig\Extension\Stopwatch',
```

Phần mở rộng Dump sẽ thay thế chức năng dump cho biến đầu ra sử dụng DataFormatter. Phần mở rộng Gỡ lỗi thêm chức năng gỡ lỗi () vượt qua các biến vào Trình thu thập tin tức, thay vì hiển thị nó trực tiếp trong mẫu. Nó bỏ các đối số, hoặc khi có sản phẩm nào; tất cả các biến ngữ cảnh.

```twig
{{ debug() }}
{{ debug(user, categories) }}
```

Phần mở rộng Stopwatch thêm một nhãn stopwatch tương tự như trong Symfony / Silex Twigbridge.

```twig
{% stopwatch "foo" %}
    …some things that gets timed
{% endstopwatch %}
```
