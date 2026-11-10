# Leitor

O checkout contém o shell web e um arquivo `leitor-projeto.zip` com o app em `leitor/`. A raiz não contém `src/` nem `android/`; o workflow `.github/workflows/...` extrai o ZIP e executa os comandos dentro de `leitor/`. Portanto, a auditoria anterior que afirmava ausência de extração no workflow estava incorreta.

## Dependências

Requer Node.js e npm (o workflow usa Node.js 20). O app empacotado usa Vite e Capacitor; a compilação Android também requer JDK, Android SDK e Gradle wrapper.

## Execução web local

No PowerShell, a partir desta pasta:

```powershell
Expand-Archive -LiteralPath .\leitor-projeto.zip -DestinationPath . -Force
Set-Location .\leitor
npm ci
npm run dev
```

Para build web: `npm run build`. Para Android, configure Android SDK e JDK, depois `npx cap sync android` e `android\gradlew.bat assembleDebug`. A extração produz `leitor/`; não execute estes comandos na raiz.

A integridade CRC do ZIP foi verificada. O fluxo CI foi inspecionado, mas não executado nesta revisão. Não foi possível executar npm localmente nesta máquina; pnpm e Bun estão disponíveis, mas ainda não validados para este lockfile.

## Privacidade

O app solicita uma chave Anthropic que fica no `localStorage` do navegador e envia texto extraído de PDFs à API Anthropic. Não use documentos sensíveis sem avaliar consentimento, retenção, custos e política do serviço; limpe a chave ao terminar. Não inclua a chave em commits.
