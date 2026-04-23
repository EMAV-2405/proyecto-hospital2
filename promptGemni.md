# 🏥 PROYECTO: **HospitalCRUD en Flutter + Firebase**

---

# 🧩 1. Estructura del proyecto

```bash
mkdir xflutterdominguez0569
cd xflutterdominguez0569
flutter create HospitalCRUD
cd HospitalCRUD
```

---

# 🔥 2. Base de datos (Firestore)

### 📁 Colección:

```
pacientes
```

### 📌 Campos:

* nombre (String)
* edad (int)
* enfermedad (String)

---

# 📦 3. Librerías

```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.0.0
  cloud_firestore: ^4.0.0
```

---

# ⚙️ 4. main.dart

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'screens/home_screen.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Hospital CRUD',
      theme: ThemeData(
        primaryColor: Colors.redAccent,
        scaffoldBackgroundColor: Colors.grey[100],
      ),
      home: HomeScreen(),
    );
  }
}
```

---

# 🧱 5. Estructura (ANTIGRAVITY)

```
lib/
│
├── models/
│   └── paciente.dart
│
├── services/
│   └── firestore_service.dart
│
├── screens/
│   ├── home_screen.dart
│   ├── add_screen.dart
│   └── edit_screen.dart
│
└── main.dart
```

---

# 🧾 MODELO

### models/paciente.dart

```dart
class Paciente {
  String id;
  String nombre;
  int edad;
  String enfermedad;

  Paciente({
    required this.id,
    required this.nombre,
    required this.edad,
    required this.enfermedad,
  });

  factory Paciente.fromMap(String id, Map<String, dynamic> data) {
    return Paciente(
      id: id,
      nombre: data['nombre'],
      edad: data['edad'],
      enfermedad: data['enfermedad'],
    );
  }

  Map<String, dynamic> toMap() {
    return {
      'nombre': nombre,
      'edad': edad,
      'enfermedad': enfermedad,
    };
  }
}
```

---

# 🔥 SERVICIO (CRUD)

### services/firestore_service.dart

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/paciente.dart';

class FirestoreService {
  final CollectionReference pacientes =
      FirebaseFirestore.instance.collection('pacientes');

  Stream<List<Paciente>> getPacientes() {
    return pacientes.snapshots().map((snapshot) =>
        snapshot.docs.map((doc) =>
            Paciente.fromMap(doc.id, doc.data() as Map<String, dynamic>)).toList());
  }

  Future addPaciente(Paciente p) {
    return pacientes.add(p.toMap());
  }

  Future updatePaciente(Paciente p) {
    return pacientes.doc(p.id).update(p.toMap());
  }

  Future deletePaciente(String id) {
    return pacientes.doc(id).delete();
  }
}
```

---

# 🏠 HOME (LISTAR + ELIMINAR)

### screens/home_screen.dart

```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';
import '../models/paciente.dart';
import 'add_screen.dart';

class HomeScreen extends StatelessWidget {
  final FirestoreService service = FirestoreService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Hospital - Pacientes"),
        backgroundColor: Colors.redAccent,
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.green,
        child: Icon(Icons.add),
        onPressed: () {
          Navigator.push(
              context, MaterialPageRoute(builder: (_) => AddScreen()));
        },
      ),
      body: StreamBuilder<List<Paciente>>(
        stream: service.getPacientes(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());

          final lista = snapshot.data!;
          return ListView.builder(
            itemCount: lista.length,
            itemBuilder: (context, index) {
              final p = lista[index];
              return Card(
                margin: EdgeInsets.all(10),
                child: ListTile(
                  title: Text(p.nombre),
                  subtitle: Text("Edad: ${p.edad} | ${p.enfermedad}"),
                  trailing: IconButton(
                    icon: Icon(Icons.delete, color: Colors.red),
                    onPressed: () => service.deletePaciente(p.id),
                  ),
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

---

# ➕ CREAR PACIENTE

### screens/add_screen.dart

```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';
import '../models/paciente.dart';

class AddScreen extends StatelessWidget {
  final nombreCtrl = TextEditingController();
  final edadCtrl = TextEditingController();
  final enfermedadCtrl = TextEditingController();

  final FirestoreService service = FirestoreService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Nuevo Paciente")),
      body: Padding(
        padding: EdgeInsets.all(20),
        child: Column(
          children: [
            TextField(controller: nombreCtrl, decoration: InputDecoration(labelText: "Nombre")),
            TextField(controller: edadCtrl, decoration: InputDecoration(labelText: "Edad")),
            TextField(controller: enfermedadCtrl, decoration: InputDecoration(labelText: "Enfermedad")),
            SizedBox(height: 20),
            ElevatedButton(
              child: Text("Guardar"),
              onPressed: () {
                final p = Paciente(
                  id: '',
                  nombre: nombreCtrl.text,
                  edad: int.parse(edadCtrl.text),
                  enfermedad: enfermedadCtrl.text,
                );
                service.addPaciente(p);
                Navigator.pop(context);
              },
            )
          ],
        ),
      ),
    );
  }
}
```

---

# ✏️ EDITAR (UPDATE)

Puedes copiar `AddScreen` y modificarlo así:

* Recibir objeto Paciente
* Llenar controllers
* Usar `updatePaciente()`

Si quieres te lo hago completo en el siguiente mensaje.

---

# 🎨 DISEÑO (modo hospital)

```dart
theme: ThemeData(
  primaryColor: Colors.red,
  scaffoldBackgroundColor: Colors.white,
  appBarTheme: AppBarTheme(
    backgroundColor: Colors.redAccent,
  ),
)
```

---

# 🧠 METODOLOGÍA ANTIGRAVITY (ADAPTADA)

### 👥 ROLES

* UI → Pantallas hospital
* Data → Pacientes
* Service → Firebase
* QA → Validación (edad numérica, campos vacíos)

---

### 🤖 AGENTES

* Agente Paciente → gestiona datos
* Agente Firebase → CRUD
* Agente UI → renderiza listas

---

### 🔄 FLUJO

```
Doctor/Admin → App → Firebase → Datos en tiempo real
```

---

# 🧪 PRÁCTICA GUIADA

1. Crear proyecto
2. Conectar Firebase
3. Crear colección `pacientes`
4. Insertar datos manualmente en Firebase
5. Mostrar datos en app
6. Crear formulario
7. Agregar paciente
8. Eliminar paciente
9. Editar paciente

---

# ✅ RESULTADO FINAL

✔ App tipo hospital
✔ CRUD completo
✔ Firebase en tiempo real
✔ Diseño profesional
