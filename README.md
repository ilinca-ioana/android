
     <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:orientation="vertical"
        android:padding="20dp"
        tools:layout_editor_absoluteX="0dp"
        tools:layout_editor_absoluteY="28dp">

        <Button
            android:id="@+id/btnSave"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/btn_save" />

        <Button
            android:id="@+id/btnSync"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/btn_sync" />

        <Button
            android:id="@+id/btnView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/btn_view" />
    </LinearLayout>
strings

        <resources>
            <string name="app_name">My Applicationedbe</string>
            <string name="btn_save">Save data</string>
            <string name="btn_sync">Sync data</string>
            <string name="btn_view">View data</string>
            <string name="msg_success">SUCCESS</string>
            <string name="msg_failure">FAILURE</string>
        </resources>
    <uses-permission android:name="android.permission.INTERNET" />
main
    
    package com.example.myapplicationedbe;
    import android.os.Bundle;
    import android.widget.Button;
    
    import androidx.activity.EdgeToEdge;
    import androidx.appcompat.app.AppCompatActivity;
    import androidx.core.graphics.Insets;
    import androidx.core.view.ViewCompat;
    import androidx.core.view.WindowInsetsCompat;
    
    public class MainActivity extends AppCompatActivity {

    private Button btnSave;
    private Button btnSync;
    private Button btnView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        btnSave = findViewById(R.id.btnSave);
        btnSync = findViewById(R.id.btnSync);
        btnView = findViewById(R.id.btnView);
        btnSync.setOnClickListener(v -> {
            syncDataFromGithub();
        });
    }
    private void syncDataFromGithub() {
        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    java.net.URL url = new java.net.URL("https://raw.githubusercontent.com/yourname/repo/main/bookings.json");
                    java.net.HttpURLConnection connection = (java.net.HttpURLConnection) url.openConnection();
                    java.io.InputStream inputStream = connection.getInputStream();
                    java.util.Scanner scanner = new java.util.Scanner(inputStream);
                    StringBuilder result = new StringBuilder();

                    while (scanner.hasNextLine()) {
                        result.append(scanner.nextLine());
                    }
                    scanner.close();
                    connection.disconnect();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}

toast

    private void syncDataFromGithub() {
        new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    java.net.URL url = new java.net.URL("https://raw.githubusercontent.com/yourname/repo/main/bookings.json");
                    java.net.HttpURLConnection connection = (java.net.HttpURLConnection) url.openConnection();
                    
                java.io.InputStream inputStream = connection.getInputStream();
                java.util.Scanner scanner = new java.util.Scanner(inputStream);
                while (scanner.hasNextLine()) {
                    scanner.nextLine(); // Just reading the data for now
                }
                scanner.close();
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        Toast.makeText(MainActivity.this, getString(R.string.msg_success), Toast.LENGTH_SHORT).show();
                    }
                });

            } catch (Exception e) {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        Toast.makeText(MainActivity.this, getString(R.string.msg_failure), Toast.LENGTH_SHORT).show();
                    }
                });
            }
        }
    }).start();
}
