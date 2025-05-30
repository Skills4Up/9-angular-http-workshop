# 1. Fundamentos de Servicios y HTTP en Angular (Guía Completa End-to-End)

Domina los fundamentos de servicios y peticiones HTTP en Angular con una visión integral:  
**Aprenderás el “por qué” y el “cómo” de la integración de servicios, HttpClient, ciclo de vida, templates, routing y formularios, usando un mock server realista y buenas prácticas profesionales.**

---

## 🎯 Objetivos del bloque

- Comprender el papel de los servicios en la arquitectura Angular.
- Saber cuándo y por qué usar servicios para la comunicación con APIs.
- Configurar y utilizar HttpClient de forma segura y eficiente.
- Diferenciar entre los enfoques de formularios en Angular y su integración con servicios.
- Aplicar buenas prácticas de diseño, organización y manejo de errores.
- Prepararse para escenarios reales y escalables en aplicaciones profesionales.

---

## 📚 Contenido del bloque

1. **Conceptos clave de servicios y HTTP en Angular**
2. **Configuración y uso de HttpClient**
3. **Patrones de consumo de APIs y ciclo de vida**
4. **Fundamentos de formularios y data binding**
5. **Buenas prácticas y errores comunes**
6. **Preguntas conceptuales y ejercicios de reflexión**

---

## 1. ¿Qué es un servicio en Angular y por qué es fundamental?

Un **servicio** en Angular es una clase que encapsula lógica de negocio o acceso a datos, separando responsabilidades y permitiendo la reutilización y el testeo.  
Los servicios son ideales para:

- Centralizar la comunicación con APIs externas (REST, GraphQL, etc.).
- Compartir datos y lógica entre componentes.
- Implementar lógica de negocio independiente de la UI.

**Ventajas de usar servicios:**

- Separación de responsabilidades: los componentes se enfocan en la presentación.
- Reutilización: un mismo servicio puede ser usado por varios componentes.
- Testabilidad: los servicios pueden ser testeados de forma aislada.
- Mantenibilidad: facilita el cambio de lógica o endpoints sin afectar la UI.

---

## 2. ¿Qué es HttpClient y cómo se integra en Angular?

**HttpClient** es el módulo oficial de Angular para realizar peticiones HTTP.  
Permite consumir APIs externas de manera reactiva, tipada y segura.

**Características principales:**

- Soporte para todos los métodos HTTP (GET, POST, PUT, DELETE, etc.).
- Integración con Observables para manejo asíncrono y reactivo.
- Tipado fuerte gracias a TypeScript.
- Manejo de errores y transformación de respuestas.
- Fácil integración con interceptores y autenticación (en temas avanzados).

**Configuración básica:**

```typescript
// Importa HttpClientModule en el módulo raíz
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [
    HttpClientModule,
    // otros módulos...
  ],
})
export class AppModule {}
```

**Uso general en un servicio:**

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class DataService {
  constructor(private http: HttpClient) {}

  getData<T>(url: string): Observable<T> {
    return this.http.get<T>(url);
  }

  createData<T>(url: string, body: T): Observable<T> {
    return this.http.post<T>(url, body);
  }

  updateData<T>(url: string, body: T): Observable<T> {
    return this.http.put<T>(url, body);
  }

  deleteData<T>(url: string): Observable<T> {
    return this.http.delete<T>(url);
  }
}
```

---

## 3. Patrones de consumo de APIs y ciclo de vida

El consumo de APIs en Angular sigue un patrón claro y profesional:

1. **Define interfaces** para tipar los datos que esperas de la API.
2. **Crea servicios** que encapsulan la lógica de acceso a datos.
3. **Usa métodos del servicio** en los componentes, generalmente en el ciclo de vida `ngOnInit`.
4. **Gestiona el estado** (cargando, error, datos) en los componentes.
5. **Integra la navegación** usando rutas para vistas de lista, detalle, creación y edición.
6. **Maneja errores** y muestra mensajes claros al usuario.

**Ejemplo agnóstico de consumo en un componente:**

```typescript
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-generic-list',
  template: `
    <div *ngIf="loading">Cargando...</div>
    <div *ngIf="error">{{ error }}</div>
    <ul *ngIf="!loading && !error">
      <li *ngFor="let item of items">{{ item | json }}</li>
    </ul>
  `
})
export class GenericListComponent implements OnInit {
  items: any[] = [];
  loading = true;
  error: string | null = null;

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.dataService.getData<any[]>('https://api.example.com/items')
      .subscribe({
        next: data => {
          this.items = data;
          this.loading = false;
        },
        error: err => {
          this.error = 'No se pudieron cargar los datos.';
          this.loading = false;
        }
      });
  }
}
```

---

## 4. Fundamentos de formularios y data binding en Angular

Angular ofrece dos enfoques para formularios:

- **Template-driven:** Declarativo, ideal para formularios simples. Usa directivas como `ngForm` y `ngModel` en el template.
- **Reactivo:** Programático, ideal para formularios complejos o dinámicos. Usa clases como `FormGroup`, `FormControl` y `FormBuilder` en el componente.

**Ejemplo agnóstico de formulario template-driven:**

```html
<form #f="ngForm" (ngSubmit)="onSubmit(f)">
  <input name="nombre" [(ngModel)]="model.nombre" required>
  <input name="email" [(ngModel)]="model.email" required email>
  <button type="submit" [disabled]="f.invalid">Enviar</button>
</form>
```

**Ejemplo agnóstico de formulario reactivo:**

```typescript
import { FormGroup, FormControl, Validators } from '@angular/forms';

form = new FormGroup({
  nombre: new FormControl('', Validators.required),
  email: new FormControl('', [Validators.required, Validators.email])
});

onSubmit() {
  if (this.form.valid) {
    // Procesar datos
  }
}
```

```html
<form [formGroup]="form" (ngSubmit)="onSubmit()">
  <input formControlName="nombre">
  <input formControlName="email">
  <button type="submit" [disabled]="form.invalid">Enviar</button>
</form>
```

**Data binding en formularios:**

- **Interpolación:** `{{ variable }}`
- **Property binding:** `[property]="variable"`
- **Event binding:** `(event)="funcion()"`
- **Two-way binding:** `[(ngModel)]="variable"`

---

## 5. Buenas prácticas y errores comunes

- **Centraliza la lógica de datos** en servicios para mantener los componentes limpios y reutilizables.
- **Usa interfaces y tipado fuerte** para todas las respuestas HTTP.
- **Maneja errores** en los servicios usando operadores como `catchError` y muestra mensajes amigables al usuario.
- **Carga datos en `ngOnInit`** y nunca en el constructor.
- **Desuscríbete de los observables** en componentes complejos usando `takeUntil` y `ngOnDestroy`.
- **Valida los formularios** antes de enviar datos a la API.
- **Usa rutas** para separar vistas y flujos.
- **Reutiliza componentes y templates** para crear y editar.
- **Mantén el código organizado**: agrupa servicios y componentes por funcionalidad.
- **Evita lógica de negocio en los componentes**: delega todo lo posible a los servicios.

---

## 6. Preguntas conceptuales y ejercicios de reflexión

1. ¿Por qué es importante separar la lógica de datos en servicios?
2. ¿Cómo se configura HttpClient en Angular y por qué solo debe importarse una vez?
3. ¿Qué ventajas tiene usar interfaces para tipar las respuestas?
4. ¿Cómo se navega entre vistas usando rutas en Angular?
5. ¿Por qué es importante validar los formularios antes de enviar datos?
6. ¿Cuándo usarías un formulario reactivo en vez de uno template-driven?
7. ¿Cómo se maneja el ciclo de vida y la limpieza de observables en Angular?
8. ¿Qué problemas puedes evitar aplicando buenas prácticas desde el inicio?

---

## 🛠️ Ejercicios prácticos sugeridos

- Configura y ejecuta un mock server con json-server y el archivo `db.json`.
- Crea un servicio Angular para consumir la API simulada y tipifica las respuestas.
- Implementa componentes para listar, ver detalle, crear y editar datos.
- Integra rutas y navegación entre vistas.
- Valida formularios y muestra mensajes de error.
- Refuerza el ciclo de vida usando `ngOnInit` y manejo de errores.
- Implementa un formulario reactivo para un caso de edición avanzada (reto).
- Refactoriza el código para aplicar todas las buenas prácticas vistas.

---

**Recuerda:**  
El dominio de servicios y HTTP en Angular no solo es cuestión de saber “cómo” hacer las cosas, sino de entender “por qué” se hacen así. Practica, reflexiona y adapta estos principios a tus propios proyectos para convertirte en un desarrollador profesional y versátil.
