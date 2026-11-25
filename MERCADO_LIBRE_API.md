# Mercado Libre API - MarketFlow AI

## Goal

Document the initial integration between MarketFlow AI and the Mercado Libre API.

The MVP goal is to receive customer questions about products, generate a suggested answer with AI, and let the user review the answer before publishing it.

## Official source

Official documentation:

https://developers.mercadolibre.com.ar/

## Initial scope

For the MVP we only need to understand:

1. OAuth authentication.
2. Customer questions.
3. Answers to questions.
4. Notifications or webhooks.
5. Querying product information.

## Initial country

```text
Country: Colombia
Initial marketplace: Mercado Libre
Automation: suggested answer with human review
```

## Key concepts

### OAuth

OAuth lets an external application, like MarketFlow AI, connect to a Mercado Libre account without asking for the user's password directly.

The application gets permissions through tokens.

### Access Token

A temporary token used to call the Mercado Libre API.

### Refresh Token

A token used to renew the access token when it expires.

### Webhook / Notification

The mechanism Mercado Libre uses to notify MarketFlow AI when an event happens, for example a new customer question.

## Expected MVP flow

1. Customer asks a question on Mercado Libre.
2. Mercado Libre notifies the event.
3. n8n receives the notification.
4. n8n fetches the question.
5. n8n queries the product information in Supabase.
6. n8n sends the context to the AI.
7. The AI generates a suggested answer.
8. The answer is stored in Supabase.
9. The user reviews, edits, approves or rejects it.
10. The answer is published only after approval.

## Data we need to store

### Authentication data

- MELI_CLIENT_ID.
- MELI_CLIENT_SECRET.
- MELI_REDIRECT_URI.
- MELI_ACCESS_TOKEN.
- MELI_REFRESH_TOKEN.

These must live in environment variables, never written directly in the code.

### Question data

- question_id.
- item_id.
- seller_id.
- customer_id.
- pregunta_texto (question text).
- fecha_pregunta (question date).
- estado (status).
- canal (channel).

### Product data

- item_id.
- nombre (name).
- descripcion (description).
- precio (price).
- moneda (currency).
- stock.
- condicion (condition).
- garantia (warranty).
- ciudad (city).
- pais (country).

### Answer data

- question_id.
- respuesta_sugerida (suggested answer).
- respuesta_final (final answer).
- estado_revision (review status).
- aprobado_por_usuario (approved by user).
- fecha_respuesta (answer date).
- modelo_ia_usado (AI model used).

## Rules for MarketFlow AI

The AI must not publish answers automatically in the first phase.

The AI must only suggest answers.

Cases that require mandatory human review:

- Prices.
- Availability.
- Warranty.
- Shipping.
- Returns.
- Complaints.
- Payments.
- Changes to commercial terms.

## To-do

- Create an application in Mercado Libre Developers.
- Get the Client ID.
- Get the Client Secret.
- Configure the Redirect URI.
- Test OAuth.
- Test fetching questions.
- Test publishing answers.
- Define the n8n flow.
