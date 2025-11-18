---
inclusion: fileMatch
fileMatchPattern: '**/*.rs'
---

# Rust Error Handling Best Practices

When working with Rust web applications (especially Axum), follow these error handling patterns to reduce boilerplate and improve code quality.

## Custom AppError Pattern

For Axum handlers, implement a custom error type that wraps `anyhow::Error`:

```rust
pub struct AppError(anyhow::Error);

impl IntoResponse for AppError {
    fn into_response(self) -> Response {
        (StatusCode::INTERNAL_SERVER_ERROR, self.0.to_string()).into_response()
    }
}

impl<E> From<E> for AppError
where
    E: Into<anyhow::Error>
{
    fn from(err: E) -> Self {
        Self(err.into())
    }
}
```

## Handler Function Pattern

Use the `?` operator instead of verbose match statements:

**DO THIS:**
```rust
async fn handler(State(state): State<AppState>) -> Result<(StatusCode, Html<String>), AppError> {
    let data = fetch_data(&state.db).await?;
    let html = template.render()?;
    Ok((StatusCode::OK, Html(html)))
}
```

**NOT THIS:**
```rust
async fn handler(State(state): State<AppState>) -> Result<(StatusCode, Html<String>), (StatusCode, String)> {
    let data = match fetch_data(&state.db).await {
        Ok(data) => data,
        Err(e) => return Err((StatusCode::INTERNAL_SERVER_ERROR, format!("Error: {e}")))
    };
    // ... more match statements
}
```

## Benefits

- Eliminates repetitive error handling boilerplate
- Automatic error type conversion via `anyhow::Error`
- Consistent HTTP error responses
- Cleaner, more readable handler functions
- Works with any error type that implements `Into<anyhow::Error>` (sqlx, serde_json, reqwest, askama, etc.)

## Dependencies

Add to `Cargo.toml`:
```toml
anyhow = "1.0"
axum = "0.8"
```

Reference: https://rup12.net/posts/learning-rust-custom-errors/
