# Guia para Agentes de IA - Caramelo Labs

## Sobre o Projeto

**Caramelo Labs** é um laboratório prático de tecnologia focado em aprendizado hands-on. O projeto usa **Astro** + **Starlight** para gerar um site de documentação publicado via GitHub Pages.

Estrutura:

- `src/content/docs/` - Anotações teóricas (publicadas no site)
- `examples/` - Exercícios e projetos práticos
- `src/styles/custom.css` - Customizações visuais

## Comandos Essenciais

```bash
npm run dev       # Servidor local em localhost:4321 (hot reload)
npm run build     # Build para produção
npm run preview   # Preview do build gerado
npm install       # Instalar dependências
```

## Convenções de Documentação

### Frontmatter Obrigatório

Todos os arquivos em `src/content/docs/` precisam deste frontmatter (nesta ordem):

```yaml
---
title: "Título da nota"
description: "Descrição breve (50-100 chars recomendado)"
lastUpdated: YYYY-MM-DD
sidebar:
  order: N # Número inteiro para controlar ordem na sidebar
tags: ["tag1", "tag2"] # Opcional
---
```

**Validação**: Falta de `title`, `description` ou `sidebar.order` quebra o build. Use `npm run dev` para validar.

### Naming

- **Prefix numérico**: `01-introducao.md`, `02-conceitos-basicos.md` (controla ordem)
- **Formato**: `NN-nome-descritivo.md` (kebab-case)
- **Extensões**: `.md` para Markdown, `.mdx` para MDX (com componentes React)

### Estrutura de Conteúdo

Organize com headings h2 (#) e h3 (##):

```markdown
# Título (h1 - apenas um por arquivo)

## Seção principal

### Subseção
```

## Tipos de Arquivo

| Tipo       | Local                   | Formato                 | Propósito                  |
| ---------- | ----------------------- | ----------------------- | -------------------------- |
| Anotações  | `src/content/docs/`     | `.md` com frontmatter   | Conteúdo teórico publicado |
| Index/Home | `src/content/docs/`     | `index.mdx`             | Página de entrada do site  |
| Exercícios | `examples/exercises.md` | Markdown com enunciados | Desafios práticos          |
| Projetos   | `examples/projects.md`  | Markdown descritivo     | Projetos maiores           |

## Padrões e Boas Práticas

### Adicionar Nova Anotação

1. Criar `src/content/docs/NN-nome.md` com frontmatter completo
2. **Incrementar `order`** corretamente (se inserir no meio, reordenar seguintes)
3. Usar h2 para seções principais, h3 para subseções
4. Linkar para `examples/exercises.md` ou `examples/projects.md` quando relevante
5. Rodar `npm run dev` para validar antes de commitar

### Links Internos

Use paths relativos ou `href` completo:

```markdown
[Exercícios](../../../examples/exercises.md)
[Voltar para docs](../01-introducao.md)
```

### Sincronização de Conteúdo

- Cada anotação em `docs/` pode ter exercícios relacionados em `examples/exercises.md`
- Projetos devem referenciar ou estender conceitos aprendidos nas anotações
- Manter consistência de termos e nomenclatura entre arquivos

## Pitfalls Comuns

⚠️ **Metadados incompletos**: Qualquer `title`, `description` ou `order` faltando quebra o build  
⚠️ **Ordem duplicada**: Múltiplos arquivos com mesmo `order` causam comportamento impredizível  
⚠️ **Reorganização sem reordenar**: Adicionar novo arquivo no meio sem ajustar `order` dos seguintes  
⚠️ **Cache stale**: Modificações em `astro.config.mjs` ou novo arquivo podem exigir `Ctrl+C` + `npm run dev` novamente  
⚠️ **Links quebrados**: Mover/renomear arquivo sem atualizar links em outros documentos

## Arquitetura

**Loader de Conteúdo**: Astro lê automaticamente `src/content/docs/` e valida contra schema em `src/content/config.ts`

**Tema Starlight**: Gera navigation, sidebar, search e styling baseado em frontmatter e estrutura de arquivos

**Deploymentе**: GitHub Actions publica build em `https://caramelotech.com.br/caramelo-labs` (branch `gh-pages`)

## Próximas Customizações Sugeridas

- [ ] **Skill para validação de frontmatter**: Verificar campos obrigatórios antes de adicionar novo conteúdo
- [ ] **Skill para atualizar `order`**: Automatizar reordenação ao inserir novo arquivo
- [ ] **Instruções para exercícios**: Padrão para enunciados estruturados e critérios de sucesso

Para sugestões ou mudanças neste guia, abra uma issue ou faça um PR.
