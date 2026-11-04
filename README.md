# TP Final — Ecosistema de Soporte Autónomo con IA y HITL

Workflow de **n8n** para recibir tickets por webhook, validarlos, guardarlos en Airtable, clasificarlos con Google Gemini, generar un borrador de respuesta y solicitar aprobación humana antes de enviar el email.

## Flujo

1. `POST /webhook/nuevo-ticket` recibe `subject`, `email` y `message`.
2. Se bloquean emails del dominio interno y mensajes vacíos.
3. El ticket se guarda en Airtable con estado `Pendiente`.
4. Gemini devuelve categoría, prioridad y borrador en JSON.
5. Airtable pasa el ticket a `En Revisión HITL`.
6. El nodo `Wait` espera la aprobación humana.
7. Se envía el borrador por SMTP y el ticket queda `Respondido`.

## Importación

Importar `workflows/Ecosistema_Soporte_Autonomo_HITL.json` desde n8n y reemplazar las credenciales por las de la instancia destino:

- Airtable Personal Access Token
- Google Gemini
- SMTP

El export está sanitizado: no contiene tokens ni secretos. Los IDs de credenciales y de recursos deben revisarse antes de activar el workflow.

## Payload de ejemplo

```json
{
  "subject": "No puedo ingresar",
  "email": "cliente@example.com",
  "message": "El sistema rechaza mi contraseña."
}
```

## Campos esperados en Airtable

`Asunto`, `Email_Remitente`, `Mensaje`, `Categoria`, `Prioridad`, `Borrador_Respuesta`, `Estado` y `Aprobado`.

