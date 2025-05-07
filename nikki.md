# 日記

## 4/26

mac 移行作業　モチベーション低下
とりあえずで Docker に移行しようとする

sudo find /usr/share/zsh -path '\*/functions/zle/choose-history' -type f -print

## 4/27

docker 移行作業本格化
frontend backend
supabase db
明日 alembic のマイグレーションや db 動作チェックなど

## 4/29

バックエンド　環境構築
database.py

## 5/5

UndefinedTableError の解決（最優先）:

---

minimal_db_test.py が正常に動作するように、まずテーブル作成の問題を解決します。
試すこと:
conftest.py の setup_test_tables 内、create_all の直後に print(f"Tables created: {Base.metadata.tables.keys()}") を追加し、staff が含まれているかログで確認します。
conftest.py の一番上（他の app 関連インポートより前）に import app.models という行を追加し、モデル定義が早期に読み込まれるように強制してみます。
app/models/staff.py を確認し、from app.db.database import Base を使って正しく Base を継承しているか再確認します。

---

InterfaceError への再挑戦:

---

UndefinedTableError が解決し、minimal_db_test.py が正常に flush できるようになったら、元の tests/api/v1/test_service_recipient.py を再度実行します。
期待: minimal_db_test.py で InterfaceError が発生しなかったことから、test_service_recipient.py でも InterfaceError が解消されている可能性があります。これは、setup_test_tables と async_db_session を分離し、依存関係を明確にした最新の conftest.py の構成が、間接的にフィクスチャ間の競合を防いだ結果かもしれません。
もし InterfaceError が再発した場合: 問題は test_staff_factory または get_auth_headers に絞られます。その場合は、これらのフィクスチャ内の await の使い方や、async_db_session とのやり取りをさらに詳細に見直す必要があります。（例: test_staff_factory 内の flush をやめてみる、など）

## 5/6

テーブル作成の問題-> モデルの sqlalchemy v2 書き方問題-> エラーの要因考察：テーブルが作成されていない? マイグレーション
-> alembic current を実行するためのエラー

## 5/7

InterfaceError -> 個別のテスト開始
進捗遅れるかも
