# wordpressをホストするサーバの構築

## deploy

development環境ではvagrantを用いて仮想マシンを立ち上げて、開発環境様のマシンを設定します。
production環境にはvpsを用意して接続するためのinventoryファイルを用意します。

### development environment

```
$ vagrant up 
$ ansible-playbook -i development site.yml
```

### production envrionment

 * group_vars/all.yamlの内容を編集する

```
---
login_user: "login_user"
mysql_root_pass: "must_rewrite"
wordpress: {
  dbname: "wp_db",
  dbuser: "wp_user",
  dbpass: "must_rewrite",
  src_uri: "https://ja.wordpress.org/latest-ja.tar.gz"
}
```

 * roles/wordpress/templates/wp-config.phpの内容を編集する

[オンラインジェネレータ](https://api.wordpress.org/secret-key/1.1/salt/)を利用して値を得る。

```
<?php
define('DB_NAME', '{{wordpress.dbname}}');
define('DB_USER', '{{wordpress.dbuser}}');
define('DB_PASSWORD', '{{wordpress.dbpass}}');
define('DB_HOST', 'localhost');
define('DB_CHARSET', 'utf8');
define('DB_COLLATE', '');

define('AUTH_KEY',         'put your unique phrase here');
define('SECURE_AUTH_KEY',  'put your unique phrase here');
define('LOGGED_IN_KEY',    'put your unique phrase here');
define('NONCE_KEY',        'put your unique phrase here');
define('AUTH_SALT',        'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT',   'put your unique phrase here');
define('NONCE_SALT',       'put your unique phrase here');

$table_prefix  = 'wp_';
define('WP_DEBUG', false);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
	define('ABSPATH', dirname(__FILE__) . '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');
```

 * virtual hostを利用する場合



## 残件

firewallの設定がなされていないのでそのためのroleを追加するべき。


