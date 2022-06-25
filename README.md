# Connecting to The Internet using Background Thread in android.
## PROGRAM
# activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <EditText
        android:id="@+id/editText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:inputType="textPersonName"

        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.103" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Download"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.263" />

    <EditText
        android:id="@+id/result"
        android:layout_width="401dp"
        android:layout_height="424dp"
        android:ems="10"
        android:gravity="start|top"
        android:inputType="textMultiLine"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="1.0" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
## MainActivity.java
```
package com.example.thread;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    EditText editText;
    static EditText result;
    Button button;
    String weburl;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        editText =findViewById(R.id.editText);
        result=findViewById(R.id.result);
        button=findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                weburl=editText.getText().toString();
                Downloadwebsite myclass=new Downloadwebsite(MainActivity.this);
                myclass.execute(weburl);

            }
        });
    }
}
```
## Downloadwebsite.java
```
package com.example.thread;

import android.app.AlertDialog;
import android.content.Context;
import android.content.DialogInterface;
import android.os.AsyncTask;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;


public class Downloadwebsite extends AsyncTask<String, Void ,String> {
    Context ctx;
    AlertDialog.Builder dialog;
   public  Downloadwebsite(Context ct)
   {
       ctx=ct;
   }
    @Override
    protected String doInBackground(String... strings) {
       String myweburl=strings[0];
        InputStream inputStream;
        try {
            URL myurl=new URL((myweburl));
            HttpURLConnection connection=(HttpURLConnection) myurl.openConnection();
            connection.setReadTimeout(100000);
            connection.setConnectTimeout(20000);
            connection.setRequestMethod("GET");
            connection.connect();

            inputStream=connection.getInputStream();
            BufferedReader reader=new BufferedReader(new InputStreamReader(inputStream));
            StringBuilder builder=new StringBuilder();
            String line;
            while ((line=reader.readLine()) !=null)
            {
                builder.append((line+"\n"));

            }
            inputStream.close();
            reader.close();
            return  builder.toString();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

    @Override
    protected void onPostExecute(String s) {
        dialog= new AlertDialog.Builder(ctx);
        dialog.setTitle("Congratulations");
        dialog.setMessage("Website Succecfully Downloaded");
        dialog.setPositiveButton("ok", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {

            }
        });
                dialog.create().show();
                MainActivity.result.setText(s);
    }
}
```
## <br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>Output
![Screenshot 2022-06-21 184801](https://user-images.githubusercontent.com/75235789/174810505-fbc90f41-6e47-465d-b56f-0722f09b6c41.jpg)
![Screenshot 2022-06-21 184835](https://user-images.githubusercontent.com/75235789/174810493-0277d031-1540-4675-baa0-3c4006601809.jpg)
![Screenshot 2022-06-21 184312](https://user-images.githubusercontent.com/75235789/174810514-0e61c14c-d775-4e6e-8e17-3d7df15fa812.jpg)
