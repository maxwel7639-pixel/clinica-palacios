# Clínica Palácio — landing page

Landing page da Clínica Palácio (estética, saúde e bem-estar — Osório/RS), implementada a
partir do design `Clinica Palacio.dc.html` do Claude Design.

Site estático: um `index.html` sem build, sem dependências e sem framework. É só publicar a
pasta.

```
index.html                 página inteira (HTML + CSS + JS embutidos)
og-clinica-palacio.jpg     imagem de compartilhamento (WhatsApp, Facebook, etc.)
fotos/                     hero e fotos dos tratamentos
fotos/equipe/              as 13 fotos da equipe
docs/GUIA-IMAGENS.html     guia de quais fotos vão em cada espaço
```

## Publicar

Qualquer host estático serve (Vercel, Netlify, GitHub Pages, hospedagem comum via FTP).
Não há passo de build — o conteúdo da pasta é o site.

Para rodar localmente:

```sh
python3 -m http.server 8000   # depois abra http://localhost:8000
```

## Editar contato e links

Telefone, mensagem inicial do WhatsApp, Instagram e Google Maps ficam num único bloco no fim
do `index.html`:

```js
var CONFIG = {
  whatsapp: '5551997900687',
  whatsappMessage: 'Olá! Vim pelo site da Clínica Palácio e gostaria de agendar um horário.',
  instagram: 'https://www.instagram.com/clinicapalacio_/',
  maps: 'https://www.google.com/maps/place/Clinica+Palacio/@-29.8908995,-50.266729,17z'
};
```

Todos os botões “Agendar”, o botão flutuante e os links do rodapé usam esses valores.

## Fotos

Cada espaço de imagem aponta para um arquivo com nome fixo. **Basta salvar o arquivo com o
nome certo dentro de `fotos/` — não precisa mexer no código.** Se o arquivo não existir, o
espaço aparece como um bloco discreto com a legenda do que deve ir ali, em vez de imagem
quebrada.

Já estão no ar (recuperadas do projeto de design):

| Arquivo | Onde aparece |
| --- | --- |
| `fotos/hero.webp` | Foto de abertura |
| `fotos/servico-estetica-facial.webp` | Tratamentos › Estética facial |
| `fotos/servico-estetica-corporal.webp` | Tratamentos › Estética corporal |
| `fotos/servico-harmonizacao-facial.webp` | Tratamentos › Harmonização facial |
| `fotos/servico-laser.webp` | Tratamentos › Laser |
| `fotos/servico-massoterapia.webp` | Tratamentos › Massoterapia |
| `fotos/servico-terapia-chinesa.webp` | Tratamentos › Terapia chinesa |
| `fotos/servico-sobrancelha.webp` | Tratamentos › Design de sobrancelha |
| `fotos/servico-nails.webp` | Tratamentos › Nails e manicure |
| `fotos/servico-nutricao.webp` | Tratamentos › Nutrição |
| `fotos/servico-vascular.webp` | Tratamentos › Médica vascular |
| `fotos/servico-tricologia.webp` | Tratamentos › Tricologia |
| `fotos/clinica-fachada.webp` | Seção "A clínica" |
| `fotos/equipe/*.webp` | As 13 profissionais |

Ainda faltam — o site já está preparado, é só salvar o arquivo:

| Arquivo a salvar em `fotos/` | Proporção / tamanho mín. | O que deve ir |
| --- | --- | --- |
| `antes-depois-1.jpg` … `antes-depois-6.jpg` | 1:1,16 · 1000×1160 | Montagens antes/depois, **com autorização da paciente** |
| `depoimento-google-1.png` … `-3.png` | 1,25:1 · 1200×960 | Prints das avaliações reais do Google |

Duas fotos entraram em resolução baixa e vale trocar quando houver original maior:
`servico-vascular.webp` (223×297, exibida a 399×380) e `servico-tricologia.webp`
(312×416). O ideal é 1000×1350.

As seções **“Antes e depois”** e **“Depoimentos”** ficam escondidas enquanto nenhuma das suas
fotos existir, e aparecem sozinhas assim que a primeira imagem for salva. Detalhes de
enquadramento por espaço estão em [`docs/GUIA-IMAGENS.html`](docs/GUIA-IMAGENS.html).

Três fotos da equipe são recortes de story e valem trocar pelo original quando houver:
Tatiane Palacio, Dra. Lizandra da Costa e Fernanda Reis.

## Implementação

Convertido do formato `.dc.html` do Claude Design para HTML/CSS/JS padrão:

- os placeholders `{{ waLink }}`, `{{ mapsLink }}`, `{{ igLink }}` viraram o bloco `CONFIG`;
- os `style-hover="…"` inline viraram regras CSS reais (`:hover`);
- o componente `<image-slot>` virou `<img>` comum com fallback para o bloco legendado;
- o FAQ com `<sc-if>` virou `<details name="faq">` nativo — abre um por vez, funciona sem JS
  e é acessível por teclado;
- a animação de entrada, o empilhamento sticky no mobile e o tilt dos cards foram portados
  para JS puro, respeitando `prefers-reduced-motion`.

As fotos da equipe foram convertidas de PNG para WebP: **3,3 MB → 190 KB**, sem mudança
visível.

Também incluídos: `lang="pt-BR"`, `<main>` e landmarks, texto alternativo em todas as
imagens, foco visível no teclado, dados estruturados `HealthAndBeautyBusiness` (schema.org)
para o Google, e a imagem de compartilhamento `og-clinica-palacio.jpg`.
