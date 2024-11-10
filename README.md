VÍDEO DE EXPLICACIÓN:

https://youtu.be/6-4Gn3EsKRg


Objetivo: El propósito es implementar los tres tipos de menús en Android: Popup, Contextual y Opciones, 
abarcando tanto la interfaz gráfica como la configuración mediante código. 
Este ejercicio te permitirá aprender a trabajar con menús en una aplicación Android, personalizarlos y 
entender su comportamiento en distintas interacciones de usuario.

1. AndroidManifest.xml
Este archivo es esencial para cualquier aplicación Android, ya que contiene la configuración de la aplicación y
define las actividades que estarán disponibles.
También, en este archivo, se indica el tema de la aplicación para que se vea con la barra de acción superior (Action Bar).

android:theme="@style/Theme.AppCompat.Light.DarkActionBar": 
Esta línea asegura que la aplicación tenga un tema que incluya una barra de acción superior con un fondo oscuro.

<intent-filter>: Define el punto de entrada de la aplicación, es decir, el MainActivity que se lanzará cuando se abra la aplicación.


![image](https://github.com/user-attachments/assets/96efa5ee-6814-4d58-ab23-e28becdd1083)



<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.menutypesapp">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppCompat.Light.DarkActionBar">


        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>



2. activity_main.xml
Este archivo es el layout principal de la actividad, donde se colocan los botones que el usuario utilizará para interactuar con los menús.
En este caso, hay dos botones: uno para mostrar el menú emergente y otro para el menú contextual.

RelativeLayout: Se utiliza un RelativeLayout para que los botones puedan ser colocados de manera flexible.

android:background="@color/background_blue": Define el color de fondo de la actividad, utilizando un color 
personalizado que se definirá en el archivo de recursos colors.xml.

Los botones tienen ID (popup_button, context_button) que les permite ser referenciados en el código Java/Kotlin.


![image](https://github.com/user-attachments/assets/482b9711-9e17-4de2-8192-906f4a1d28e7)


<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/background_blue">
    <Button
        android:id="@+id/popup_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Menú Popup"
        android:layout_centerInParent="true" />

    <Button
        android:id="@+id/context_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Menú Contextual"
        android:layout_below="@id/popup_button"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp" />

</RelativeLayout>





3. MainActivity.kt
Este es el archivo de actividad principal, donde se define la lógica para mostrar los menús cuando el usuario interactúa con los botones. 
Aquí, también se maneja el evento de largo clic para el menú contextual.

showPopupMenu: Esta función maneja la lógica para mostrar el menú emergente cuando 
el usuario hace clic en el botón correspondiente.

onCreateOptionsMenu: Se infla el menú de opciones en la barra superior (Action Bar).

onContextItemSelected: Maneja las opciones seleccionadas del menú contextual.


![image](https://github.com/user-attachments/assets/b2ca1f1e-1aa4-432a-9434-5f41b38dc487)

![image](https://github.com/user-attachments/assets/e9a972d8-e824-4c18-829e-7bc0f85c208b)

![image](https://github.com/user-attachments/assets/14e8099d-bd47-4987-a6f1-e4c301133d47)


package com.example.menutypesapp

import android.os.Bundle
import android.view.Menu
import android.view.MenuInflater
import android.view.MenuItem
import android.view.View
import android.widget.Button
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.appcompat.widget.PopupMenu

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)


        val popupButton: Button = findViewById(R.id.popup_button)
        popupButton.setOnClickListener {
            showPopupMenu(it)
        }


        val contextButton: Button = findViewById(R.id.context_button)


        registerForContextMenu(contextButton)


        contextButton.setOnLongClickListener { view ->
            openContextMenu(view)
            true
        }
    }

    // Método para mostrar el Menú de Opciones
    override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        val inflater: MenuInflater = menuInflater
        inflater.inflate(R.menu.options_menu, menu) // Asegúrate de que el XML esté bien configurado
        return true
    }


    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            R.id.menu_item_1 -> Toast.makeText(this, "Guardado", Toast.LENGTH_SHORT).show()
            R.id.menu_item_2 -> Toast.makeText(this, "Eliminado", Toast.LENGTH_SHORT).show()
            else -> return super.onOptionsItemSelected(item)
        }
        return true
    }


    private fun showPopupMenu(view: View) {
        val popupMenu = PopupMenu(this, view)
        val inflater = popupMenu.menuInflater
        inflater.inflate(R.menu.popup_menu, popupMenu.menu)
        popupMenu.setOnMenuItemClickListener { item ->
            when (item.itemId) {
                R.id.popup_item_1 -> {
                    Toast.makeText(this, "Editado", Toast.LENGTH_SHORT).show()
                    true
                }
                R.id.popup_item_2 -> {
                    Toast.makeText(this, "Revertido", Toast.LENGTH_SHORT).show()
                    true
                }
                else -> false
            }
        }
        popupMenu.show()
    }


    override fun onCreateContextMenu(menu: android.view.ContextMenu?, v: View?, menuInfo: android.view.ContextMenu.ContextMenuInfo?) {
        super.onCreateContextMenu(menu, v, menuInfo)
        val inflater: MenuInflater = menuInflater
        inflater.inflate(R.menu.context_menu, menu)
    }

    
    override fun onContextItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            R.id.context_menu_item_1 -> Toast.makeText(this, "Haz Continuado", Toast.LENGTH_SHORT).show()
            R.id.context_menu_item_2 -> Toast.makeText(this, "Haz Regresado", Toast.LENGTH_SHORT).show()
            else -> return super.onContextItemSelected(item)
        }
        return true
    }
}


4. options_menu.xml, popup_menu.xml y context_menu.xml
Estos archivos definen las opciones de cada uno de los menús.
En primera instancia en la carpeta res se creara un Android Resource directory llamado meo y ahi deberan estar cololocados los 3 menus

![image](https://github.com/user-attachments/assets/c10a7103-040b-4ec0-ac85-f3ea4d2da6d0)


options_menu.xml:

menu_item_1 y menu_item_2: 

Definen dos opciones en el menú de opciones de la barra superior, cada una con un ícono asociado.

![image](https://github.com/user-attachments/assets/cfcc417e-4b3f-4d2a-911e-edfa580cad19)


<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/menu_item_1"
        android:title="Guardar "
        android:icon="@drawable/ic_option_1" />
    <item
        android:id="@+id/menu_item_2"
        android:title="Eliminar "
        android:icon="@drawable/ic_option_2" />
</menu>


popup_menu.xml:

popup_item_1 y popup_item_2: 

Definen dos opciones que aparecen en el menú emergente cuando se hace clic en el botón correspondiente.

![image](https://github.com/user-attachments/assets/29b3c6bc-fcc0-419a-965e-1b854e10e848)


<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/popup_item_1"
        android:title="Editar"
        android:icon="@drawable/ic_popup_1" />
    <item
        android:id="@+id/popup_item_2"
        android:title="Deshacer"
        android:icon="@drawable/ic_popup_2" />
</menu>


context_menu.xml:

context_menu_item_1 y context_menu_item_2: 

Estas opciones aparecen en el menú contextual cuando el usuario hace un largo clic en el botón.

![image](https://github.com/user-attachments/assets/4422f719-90fd-4eeb-bf57-db0fcdd9cb74)



<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/context_menu_item_1"
        android:title="Continuar"
        android:icon="@drawable/ic_context_1" />
    <item
        android:id="@+id/context_menu_item_2"
        android:title="Regresar"
        android:icon="@drawable/ic_context_2" />
</menu>

5. colors.xml
Define los colores personalizados de la aplicación, como el fondo.

![image](https://github.com/user-attachments/assets/803edb32-a865-43b3-b627-044acd04ff52)


<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="black">#FF000000</color>
    <color name="background_blue">#21B1F3</color>
</resources>



Y por ultimo este es el resultado final: 



![Grabación-2024-11-10-084011](https://github.com/user-attachments/assets/29d5dcec-ff47-4321-91c5-6a620c365257)

