# 📘 Simulação de Colaboração no Git

Este guia mostra um **roteiro prático** de como simular colaboração entre três usuários fictícios (**Bob, Alice e João**) usando **Git** e **GitHub**, incluindo **criação de repositório**, **clonagem**, **branches** e **resolução de conflitos**.

---

## 1. Bob cria o projeto e o repositório remoto

```bash
# Criar e acessar a pasta do Bob
mkdir bob
cd bob

# Criar repositório local
git init

# Criar arquivo inicial
echo "O Senhor dos Anéis" > livros.txt
echo "A Revolução dos Bichos" >> livros.txt

git add livros.txt
git commit -m "Bob adiciona primeiros livros"

# Conectar ao repositório remoto (já criado no GitHub)
git remote add origin git@github.com:carvalhomaykon/git-exemplos.git

# Definir main e enviar para o GitHub
git branch -M main
git push -u origin main
```

---

## 2. Alice clona o repositório e faz alterações

```bash
# Clonar repositório e acessar
cd ..
git clone git@github.com:carvalhomaykon/git-exemplos.git alice
cd alice

# Alice adiciona mais livros
echo "Código Limpo" >> livros.txt
echo "Use a Cabeça Java" >> livros.txt

git add .
git commit -m "Alice adiciona mais livros"

# Criar branch e enviar
git checkout -b alice-livros
git push origin alice-livros
```

➡️ No GitHub, **Alice abre um Pull Request** para Bob revisar.

---

## 3. João clona e envia alteração

```bash
cd ..
git clone git@github.com:carvalhomaykon/git-exemplos.git joao
cd joao

# João adiciona mais um livro
echo "Memórias Póstumas de Brás Cubas" >> livros.txt

git add .
git commit -m "João adiciona um livro de Machado de Assis"

# Criar branch e enviar
git checkout -b joao-livros
git push origin joao-livros
```

➡️ No GitHub, **João abre outro Pull Request** para Bob revisar.

---

## 4. Bob cria branch para adicionar valores

```bash
cd ../bob
git checkout -b preco-livros

# Editar o arquivo com valores
nano livros.txt

git add .
git commit -m "Bob adiciona valores dos livros"
git push origin preco-livros
```

---

## 5. Conflito de Merge (Alice vs Bob)

```bash
cd ../alice
git checkout alice-livros

# Alice altera o valor do "Código Limpo"
nano livros.txt   # mudar de R$ 60,00 → R$ 80,00

git add .
git commit -m "Alice altera valores de livros"
git push origin alice-livros
```

➡️ Agora, quando tentar mesclar na main(mais atualizado):

```bash
cd ..
git checkout main
git merge alice-livros
```

O Git mostrará **conflito** em `livros.txt`.

---

## 6. Resolvendo o Conflito

1. Abrir o arquivo `livros.txt`, que estará assim:

   ```txt
   <<<<<<< HEAD
   Código Limpo - R$60,00
   =======
   Código Limpo - R$80,00
   >>>>>>> alice-livros
   ```

2. Escolher a versão correta (ou editar manualmente, ex. `Código Limpo - R$70,00`).

3. Remover os marcadores `<<<<<<<`, `=======`, `>>>>>>>`.

Depois finalize o merge:

```bash
git add livros.txt
git commit -m "Resolve conflito de preço nos livros"
```

---
