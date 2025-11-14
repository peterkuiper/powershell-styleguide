# 14. Testing

All non‑trivial modules MUST have automated tests.

## 14.1 Pester

- Use **Pester** for unit and integration tests.
- Tests MUST live in a dedicated `tests/` folder.
- Test names MUST describe behavior, not implementation details.

```powershell
Describe 'Get-Thing' {
    It 'returns a thing when it exists' {
        Get-Thing -Name 'Example' | Should -Not -BeNullOrEmpty
    }
}
```
