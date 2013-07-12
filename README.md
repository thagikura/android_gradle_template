Android Studio(+gradle) 用のプロジェクトの雛形です。

環境変数 ANDROID_HOME に sdk のパスを設定した上で使用することを前提としています。

    export ANDROID_HOME=/Applications/Android\ Studio.app/sdk

Android Studio に取り込む際は、 File -> Import Project... から Gradle のプロジェクトとして取り込んでください。

私が修正した部分のライセンスは Apache License 2.0 とします。

事前に以下の準備をしておくこと。

* ActionBarSherlock をローカルの maven リポジトリにインストールしておく。手順は以下のとおり

適当なディレクトリに ActionBarSherlock を clone する(4.3.2 以降が必要だが、まだリリースされてないので dev ブランチを使用)。

    git clone git://github.com/JakeWharton/ActionBarSherlock.git -b dev

以下の様な修正をしてローカルリポジトリに入れて( gradle :actionbarsherlock:uploadArchives )使用している。SDK Manager で、 Android 4.0(API 14) をダウンロードしておく必要が有る。

```
diff --git a/actionbarsherlock/build.gradle b/actionbarsherlock/build.gradle
index 5b05162..c05924d 100644
--- a/actionbarsherlock/build.gradle
+++ b/actionbarsherlock/build.gradle
@@ -1,7 +1,7 @@
 apply plugin: 'android-library'

 dependencies {
-  compile 'com.google.android:support-v4:r7'
+  compile 'com.android.support:support-v4:13.0.0'
 }

 android {
@@ -16,3 +16,13 @@ android {
     }
   }
 }
+
+apply plugin: 'maven'
+
+uploadArchives {
+    repositories {
+        mavenDeployer {
+            repository(url: "file://$System.env.HOME/.m2/repository")
+        }
+    }
+}
```
