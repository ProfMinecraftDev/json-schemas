# 🧩 JSON Schemas — ProfMinecraftDev

Repositorio de esquemas `.schema.json` para validación estructural.  
Sin licencia. Sin permisos. Sin garantías. Úsalo bajo tu propio riesgo.

---

## 🛠️ Validación en C# — Código Completo

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Schema;

class Program
{
    static void Main()
    {
        string schemaPath = "settings.schema.json";
        string jsonPath = "settings.json";

        if (!File.Exists(schemaPath) || !File.Exists(jsonPath))
        {
            Console.WriteLine("❌ Archivos no encontrados.");
            return;
        }

        string schemaText = File.ReadAllText(schemaPath);
        string jsonText = File.ReadAllText(jsonPath);

        JSchema schema = JSchema.Parse(schemaText);
        JObject json = JObject.Parse(jsonText);

        IList<string> errors;
        bool isValid = json.IsValid(schema, out errors);

        if (isValid)
        {
            Console.WriteLine("✅ JSON válido.");
        }
        else
        {
            Console.WriteLine("❌ JSON inválido:");
            foreach (var error in errors)
                Console.WriteLine($"   - {error}");
        }
    }
}
```

> Requiere el paquete NuGet: `Newtonsoft.Json.Schema`

---

## 📜 settings.schema.json — Ejemplo Básico

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Configuración",
  "type": "object",
  "required": ["nombre", "version", "debug"],
  "properties": {
    "nombre": {
      "type": "string",
      "minLength": 1
    },
    "version": {
      "type": "string",
      "pattern": "^\\d+\\.\\d+\\.\\d+$"
    },
    "debug": {
      "type": "boolean"
    }
  },
  "additionalProperties": false
}
```

---

## 🧾 Filosofía

- Cada `.json` debe pasar por el ritual de validación.
- No se aceptan claves huérfanas ni valores sin sentido.
- Este repositorio no perdona errores silenciosos.
