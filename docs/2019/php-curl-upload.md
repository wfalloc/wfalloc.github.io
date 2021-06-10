
#### PHP CURL 上传文件

```php
public function curlUploadFile()
{
    $url = "http://test/upload";
    $filePath = '../public/test.jpg';

    /*
     * <=5.4 curl上传文件只支持@语法
     * =5.5  支持@语法和CURLFile类
     * >=5.6 只支持CURLFile类
     */
    $ch = curl_init();
    // 用特性检测判断php版本
    if (class_exists('\CURLFile')) {
        curl_setopt($ch, CURLOPT_SAFE_UPLOAD, true);
        $data = ['file' => new \CURLFile(realpath($filePath))]; // >=5.5
    } else {
        if (defined('CURLOPT_SAFE_UPLOAD')) {
            curl_setopt($ch, CURLOPT_SAFE_UPLOAD, false);
        }
        $data = ['file' => '@' . realpath($filePath)]; // <=5.5
    }
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
    $result = curl_exec($ch);
    $error  = curl_error($ch);
    curl_close($ch);
    return $error ? $error : $result;
}
```

---
date: 2019-10-23 11:45:46