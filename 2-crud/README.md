# 2. Operaciones CRUD y Manejo de Errores en Angular (Guía Práctica y Profesional)

Domina la manipulación de datos en Angular implementando operaciones CRUD (Crear, Leer, Actualizar, Eliminar) y el manejo profesional de errores HTTP.  
**Aprenderás a estructurar servicios, componentes y formularios para flujos reales, integrando buenas prácticas y ejemplos end-to-end.**

---

## 🎯 Objetivos del bloque

- Comprender el ciclo completo de operaciones CRUD en Angular.
- Implementar servicios y componentes para manipular datos de una API.
- Integrar formularios para crear y editar datos.
- Manejar errores HTTP de forma profesional y mostrar mensajes claros al usuario.
- Aplicar buenas prácticas de organización, tipado y manejo de estado.
- Prepararse para escenarios reales y escalables en aplicaciones profesionales.

---

## 📚 Contenido del bloque

1. **Operaciones CRUD con Servicios y HttpClient**
2. **Manejo de Errores HTTP**
3. **Buenas Prácticas en Servicios y Consumo de APIs**
4. **Preguntas conceptuales y ejercicios de reflexión**

---

## 1. ¿Qué es CRUD y por qué es esencial en Angular?

CRUD representa las operaciones básicas para manipular datos en cualquier aplicación:

- **C**rear (Create): Agregar nuevos registros.
- **R**ecuperar (Read): Consultar y mostrar datos.
- **U**pdate (Actualizar): Modificar registros existentes.
- **D**elete (Eliminar): Borrar registros.

En Angular, estas operaciones se implementan usando servicios y el módulo HttpClient, permitiendo que los componentes interactúen con APIs externas de manera profesional y mantenible.

---

## 2. Implementación de CRUD en Angular

**Patrón recomendado:**

1. Define interfaces para tipar los datos.
2. Centraliza la lógica de acceso a datos en servicios.
3. Usa métodos del servicio en los componentes.
4. Integra formularios para crear y editar datos.
5. Gestiona el estado (cargando, error, datos) en los componentes.
6. Navega entre vistas usando rutas.
7. Actualiza la UI tras cada operación.

**Ejemplo agnóstico de servicio CRUD:**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class CrudService<T> {
  constructor(private http: HttpClient, private apiUrl: string) {}

  getAll(): Observable<T[]> {
    return this.http.get<T[]>(this.apiUrl);
  }

  getById(id: number): Observable<T> {
    return this.http.get<T>(`${this.apiUrl}/${id}`);
  }

  create(item: T): Observable<T> {
    return this.http.post<T>(this.apiUrl, item);
  }

  update(id: number, item: T): Observable<T> {
    return this.http.put<T>(`${this.apiUrl}/${id}`, item);
  }

  delete(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`);
  }
}
```

---

## 3. Manejo de Errores HTTP

El manejo de errores es fundamental para la robustez y experiencia de usuario.  
Angular permite capturar y gestionar errores usando operadores de RxJS como `catchError`.

**Patrón recomendado:**

- Captura errores en los servicios.
- Muestra mensajes claros y útiles en la UI.
- No expongas detalles internos del servidor.
- Permite reintentos o acciones alternativas cuando sea posible.

**Ejemplo agnóstico de manejo de errores:**

```typescript
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

getAll(): Observable<T[]> {
  return this.http.get<T[]>(this.apiUrl).pipe(
    catchError(error => {
      // Log interno y mensaje amigable
      return throwError(() => new Error('No se pudieron cargar los datos.'));
    })
  );
}
```

---

## 4. Buenas Prácticas en Servicios y Consumo de APIs

- Centraliza la lógica de datos en servicios, no en componentes.
- Usa interfaces y tipado fuerte para todas las respuestas HTTP.
- Maneja errores en los servicios y muestra mensajes amigables.
- Carga datos en `ngOnInit`, nunca en el constructor.
- Desuscríbete de los observables en componentes complejos.
- Valida los formularios antes de enviar datos a la API.
- Usa rutas para separar vistas y flujos.
- Reutiliza componentes y templates para crear y editar.
- Mantén el código organizado y modular.

---

## 5. Preguntas conceptuales y ejercicios de reflexión

1. ¿Por qué es importante centralizar la lógica CRUD en servicios?
2. ¿Cómo se maneja un error HTTP en Angular y por qué es importante mostrar mensajes claros?
3. ¿Qué ventajas aporta el tipado fuerte en las operaciones CRUD?
4. ¿Cómo se asegura la actualización de la UI tras una operación de creación, edición o eliminación?
5. ¿Cuándo es recomendable reutilizar un formulario para crear y editar datos?
6. ¿Qué problemas puedes evitar aplicando buenas prácticas desde el inicio?

---

## 🛠️ Ejercicios prácticos sugeridos

- Implementa un servicio CRUD genérico y úsalo en varios componentes.
- Crea formularios para crear y editar datos, validando los campos antes de enviar.
- Simula errores HTTP (apagando el mock server, enviando datos inválidos) y maneja los mensajes en la UI.
- Refactoriza componentes para aplicar buenas prácticas de organización y manejo de estado.
- Integra rutas para vistas de lista, detalle, creación y edición.
- Desuscríbete correctamente de observables en componentes que se destruyen.

---

## 🚀 Siguiente paso

Continúa con el bloque de [Integración y Casos Reales](../3-integracion/README.md) para aprender a combinar servicios, formularios, rutas y APIs externas en flujos profesionales y proyectos completos.

---

**Recuerda:**  
El dominio de las operaciones CRUD y el manejo de errores en Angular es clave para construir aplicaciones robustas y profesionales. Practica, reflexiona y adapta estos principios a tus propios proyectos para convertirte en un desarrollador versátil y confiable.
