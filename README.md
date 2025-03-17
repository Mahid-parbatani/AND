1. Pass Data to Another Application using Intent
package com.example.senddata

import android.content.Intent
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        btnSend.setOnClickListener {
            val sendIntent = Intent().apply {
                action = Intent.ACTION_SEND
                putExtra(Intent.EXTRA_TEXT, editText.text.toString())
                type = "text/plain"
            }
            startActivity(Intent.createChooser(sendIntent, "Share via"))
        }
    }
2. Display Toast on Back Button Press
3. package com.example.backbuttontoast

import android.os.Bundle
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    override fun onBackPressed() {
        Toast.makeText(this, "Back button pressed!", Toast.LENGTH_SHORT).show()
    }
} 
3. Pass Data Between Activities using Intent

First Activity
package com.example.passdata

import android.content.Intent
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        btnSend.setOnClickListener {
            val intent = Intent(this, SecondActivity::class.java)
            intent.putExtra("message", editText.text.toString())
            startActivity(intent)
        }
    }
}
Second Activity
package com.example.passdata

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import kotlinx.android.synthetic.main.activity_second.*

class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_second)

        val message = intent.getStringExtra("message")
        textView.text = message
    }
}
4. Display Selected Game from ListView
package com.example.gamelist

import android.os.Bundle
import android.widget.ArrayAdapter
import android.widget.ListView
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val games = arrayOf("PUBG", "Free Fire", "Call of Duty", "Fortnite", "Apex Legends", "Clash Royale", "Minecraft", "FIFA", "Asphalt 9", "Among Us")
        val listView: ListView = findViewById(R.id.listView)
        val textView: TextView = findViewById(R.id.textView)

        listView.adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, games)

        listView.setOnItemClickListener { _, _, position, _ ->
            textView.text = "Selected Game: ${games[position]}"
        }
    }
}
5. Detect Airplane Mode ON/OFF using Broadcast Receiver
xml:
<receiver android:name=".AirplaneModeReceiver">
    <intent-filter>
        <action android:name="android.intent.action.AIRPLANE_MODE" />
    </intent-filter>
</receiver>
kotlin:
package com.example.airplanemode

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.widget.Toast

class AirplaneModeReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        if (intent?.action == Intent.ACTION_AIRPLANE_MODE_CHANGED) {
            val isAirplaneModeOn = intent.getBooleanExtra("state", false)
            val message = if (isAirplaneModeOn) "Airplane Mode ON" else "Airplane Mode OFF"
            Toast.makeText(context, message, Toast.LENGTH_SHORT).show()
        }
    }
}
6. Play Audio using Media API
package com.example.mediaplayer

import android.media.MediaPlayer
import android.os.Bundle
import android.widget.Button
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private lateinit var mediaPlayer: MediaPlayer

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        mediaPlayer = MediaPlayer.create(this, R.raw.sample_audio)

        findViewById<Button>(R.id.btnPlay).setOnClickListener {
            if (!mediaPlayer.isPlaying) mediaPlayer.start()
        }

        findViewById<Button>(R.id.btnPause).setOnClickListener {
            if (mediaPlayer.isPlaying) mediaPlayer.pause()
        }
    }

    override fun onDestroy() {
        super.onDestroy()
        mediaPlayer.release()
    }
}
7. Submenu with Toast Message
xml:
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:title="Options">
        <menu>
            <item android:id="@+id/subitem1" android:title="Submenu Item 1" />
            <item android:id="@+id/subitem2" android:title="Submenu Item 2" />
        </menu>
    </item>
</menu>
kotlin:
package com.example.submenu

import android.os.Bundle
import android.view.Menu
import android.view.MenuInflater
import android.view.MenuItem
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    override fun onCreateOptionsMenu(menu: Menu): Boolean {
        val inflater: MenuInflater = menuInflater
        inflater.inflate(R.menu.menu, menu)
        return true
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            R.id.subitem1 -> Toast.makeText(this, "Submenu Item 1 Selected", Toast.LENGTH_SHORT).show()
            R.id.subitem2 -> Toast.makeText(this, "Submenu Item 2 Selected", Toast.LENGTH_SHORT).show()
        }
        return super.onOptionsItemSelected(item)
    }
}

