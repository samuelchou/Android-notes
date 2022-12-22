# Auto Migration

{% embed url="https://medium.com/androiddevelopers/room-auto-migrations-d5370b0ca6eb" %}

{% embed url="https://codewithvarun.com/migrating-room-db-using-auto-migrations" %}
這篇比較推薦
{% endembed %}



Automated-Migration 是 Room 2.4.0-alpha01 以後出現的新功能。這允許使用者可以不用為了簡單的更改每次都寫繁複的 Migration 與對應 SQL 指引。

目前官方承諾可以自動處理的項目包含：新增欄位，新增表格，更新 Primary Key, 修改預設值等。

配合簡單敘述（標記子宣告），還能擴充處理：刪除或重命名欄位，刪除或重命名表格等。

## 實作

分為幾個步驟：

1. 設定依賴庫
2. Database 預先設置
3. 設置 Automated-Migration
4. 撰寫測試

### 設定依賴庫

必須使用 Room 2.4.0-alpha01 以上的版本：

```
roomVersion = "2.4.1"

kapt "androidx.room:room-compiler:$roomVersion"
implementation "androidx.room:room-ktx:$roomVersion"
```

### Database 預先設置

Database 必須設置 `exportSchema` 才能使用此功能。

```kotlin
@Database(
    version = 1,
    entities = [MyItem::class],
    exportSchema = true // IMPORTANT
)
abstract class MyItemDatabase : RoomDatabase {
}
```

設置 exportSchema 可能會需要連帶設置 Schema export directory, 否則可能會跳出編譯錯誤： error: Schema export directory is not provided to the annotation processor so we cannot import the schema.

設置方法如下：需要到 build.gradle(app):

```groovy
android {
    // ...
}

kapt {
    arguments {
        arg("room.schemaLocation", "$projectDir/schemas") // or change to other way if you want.
    }
}
```

{% hint style="warning" %}
若你是第一次設置 exportSchema, **請先在設置上述完畢後執行 Make Project** / Rebuild Project 以生成 schema.json 檔，再進行下一步。

Room 需要此檔案來進行 Automated Migration. 如果直接進行下一步，可能會出現編譯錯誤：  error: Schema '(version).json' required for migration was not found at the schema out folder: (...). Cannot generate auto migrations.
{% endhint %}

### 設置 Automated Migration

首先做一些你會需要 Migration 的變更。（例如在底層 data class 加上新的欄位）

然後，回到 Database 處，開始進行改造：

```kotlin
@Database(
    version = 2,
    entities = [MyItem::class],
    autoMigrations = [AutoMigration(from = 1, to = 2)],
    exportSchema = true
)
abstract class MyItemDatabase : RoomDatabase {
}
```

























