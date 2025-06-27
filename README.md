# PHP bKash Payment Gateway

[![Packagist Version](https://img.shields.io/packagist/v/your-vendor/your-package.svg)](https://packagist.org/packages/your-vendor/your-package) [![License](https://img.shields.io/packagist/l/your-vendor/your-package.svg)](LICENSE) [![Build Status](https://img.shields.io/travis/your-vendor/your-package.svg)](https://travis-ci.org/your-vendor/your-package)

A simple, lightweight PHP library for integrating the bKash REST payment gateway in your PHP projects.

---

## Features
* Create Token
* Refresh Token
* Create & execute payments
* Handle payment callbacks
* Issue refunds
* Query transaction status
* Sandbox & production modes

## Requirements

* PHP 7.4+
* cURL extension enabled
* Composer

## Installation

Install via Composer:

```bash
composer require wmsn-web/BkashClient
```

Then in your PHP code, initialize the client:

```php
use WsmnWeb\BkashClient;

$bkash = new BkashClient([
    "base_url" => "https://tokenized.sandbox.bka.sh/v1.2.0-beta/tokenized",
    "username" => "YOU_BKASH_USERNAME",
    "password" => "BKASH_PASSWORD",
    "app_key" => "BKASH_APP_KEY",
    "app_secret" => "BKASH_APP_SECRET_KEY",
    "type"      =>"sandbox" // or 'prod'
]);
```
OR

```php
$credentials = array(
    "base_url" => "https://tokenized.sandbox.bka.sh/v1.2.0-beta/tokenized",
    "username" => "YOU_BKASH_USERNAME",
    "password" => "BKASH_PASSWORD",
    "app_key" => "BKASH_APP_KEY",
    "app_secret" => "BKASH_APP_SECRET_KEY",
    "type"      =>"sandbox" // or 'prod'
);

$bkash = new BkashClient($credentials);
```

## Usage Examples

### 1. Grant Token

Grant Token API provides an access token as a response. This token can be used to access other bKash APIs as authorization parameter. Once the token gets expired, new token can be generated through Refresh Token API.

```php
$response = $bkash->GrantToken($credentials);
```
Sample Response
<pre> ```http HTTP/1.1 200 OK Content-Type: application/json ``` ```json { "token_type": "Bearer", "id_token": "test_id_token_value", "expires_in": 3600, "refresh_token": "test_refresh_token_value" } ``` </pre>

### 1. Create a Payment

```php
$response = $bkash->createPayment([
    'amount'               => '100.00',
    'currency'             => 'BDT',
    'merchantInvoiceNumber'=> 'INV-12345',
]);
```

### 2. Execute the Payment

```php
$response = $bkash->executePayment([
    'paymentID'   => $response['paymentID'],
]);
```

### 3. Query Transaction Status

```php
$response = $bkash->queryPaymentStatus([
    'paymentID' => 'PAYMENT_ID_FROM_CREATE',
]);
```

### 4. Refund a Payment

```php
$response = $bkash->refundPayment([
    'paymentID' => 'PAYMENT_ID',
    'amount'    => '50.00',
    'trxID'     => 'TRANSACTION_ID',
    'sku'       => 'REFUND-01',
]);
```

## Configuration Options

| Option       | Type   | Default     | Description                     |
| ------------ | ------ | ----------- | ------------------------------- |
| `app_key`    | string | `null`      | Your bKash API App Key          |
| `app_secret` | string | `null`      | Your bKash API App Secret       |
| `username`   | string | `null`      | API username                    |
| `password`   | string | `null`      | API password                    |
| `base_url`   | string | Sandbox URL | Base URL for bKash endpoints    |
| `mode`       | string | `sandbox`   | `sandbox` or `production`       |
| `timeout`    | int    | `30`        | HTTP request timeout in seconds |

## Running Tests

This library uses PHPUnit. To run tests:

```bash
composer install
./vendor/bin/phpunit --configuration phpunit.xml
```

> **Tip:** Copy `phpunit.xml.dist` to `phpunit.xml` and update with your sandbox credentials.

## Examples Directory

See the `examples/` directory for full scripts:

* `create_payment.php`
* `execute_payment.php`
* `refund_payment.php`
* `query_status.php`

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AwesomeFeature`)
3. Commit your changes (`git commit -m 'Add some feature'`)
4. Push to branch (`git push origin feature/AwesomeFeature`)
5. Open a Pull Request

Please follow PSR-12 coding standards and ensure all tests pass.

## Troubleshooting

* **Authentication errors**: Double-check your `app_key`, `app_secret`, `username`, and `password`.
* **Timeouts**: Increase the `timeout` option if requests hang.
* **Sandbox vs Production**: Ensure `base_url` matches the environment.

## License

This library is released under the [MIT License](LICENSE).
