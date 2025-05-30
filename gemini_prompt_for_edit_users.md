フラスクでアプリケーションを作っています

以下のようなデータベース構造に対応するユーザー情報管理ページをHTML+CSS + JSで1つのファイルで出力して


ユーザー作成、有効化、無効化、削除の機能で作成して。

ユーザー作成時に、パスワードが見えるようにして

ユーザー編集機能は不要です。

ヘッダーに「ComPAS ユーザー情報管理」のようなタイトルと、ログアウト、マイページに戻るモーダルウインドウが表示されるハンバーガーボタンを配置して。

データベース検索機能を追加して。列で絞り込みができるようにして、検索結果は複数行選択できるようにして、

ユーザー情報の横方向に結合する形で最終プレイ日時とプレイ回数などが表示されるようにして

各列、昇順、降順で並び替えができるようにして



一般的なデータベース画面にある要素は積極的に追加して

その他 改良点あれば、積極的に改良して



mysql> show create table users\G

*************************** 1. row ***************************

       Table: users

Create Table: CREATE TABLE `users` (

  `id` int NOT NULL AUTO_INCREMENT,

  `email` varchar(255) NOT NULL,

  `name` varchar(100) NOT NULL,

  `password` varchar(255) NOT NULL,

  `role` varchar(100) DEFAULT NULL,

  `is_active` tinyint(1) NOT NULL DEFAULT '1',

  `last_login_at` datetime DEFAULT NULL,

  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,

  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,

  `deleted_at` datetime DEFAULT NULL,

  PRIMARY KEY (`id`),

  UNIQUE KEY `email` (`email`)

) ENGINE=InnoDB AUTO_INCREMENT=26 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci COMMENT='flaskアプリ(flask-app)のユーザー情報を定義したテーブル'

1 row in set (0.00 sec)



mysql> show create table question_results\G

*************************** 1. row ***************************

       Table: question_results

Create Table: CREATE TABLE `question_results` (

  `id` bigint unsigned NOT NULL AUTO_INCREMENT COMMENT '内部 ID',

  `user_id` int NOT NULL COMMENT 'users.id 想定',

  `question_id` bigint unsigned NOT NULL COMMENT 'questionList.id 想定',

  `status` enum('in_progress','completed','paused','timeout','error') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'in_progress',

  `started_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,

  `submitted_at` datetime DEFAULT NULL,

  `partnership_score` smallint DEFAULT NULL,

  `partnership_text` text COLLATE utf8mb4_unicode_ci,

  `summary_text` text COLLATE utf8mb4_unicode_ci,

  `advice_points` smallint DEFAULT NULL,

  `advice_text` text COLLATE utf8mb4_unicode_ci,

  `turn_limits` smallint unsigned NOT NULL DEFAULT '10' COMMENT '許可ターン数 (0〜999)',

  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,

  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,

  PRIMARY KEY (`id`),

  KEY `idx_user_question` (`user_id`,`question_id`),

  KEY `idx_status` (`status`)

) ENGINE=InnoDB AUTO_INCREMENT=140 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci

1 row in set (0.01 sec)