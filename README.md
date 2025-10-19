# Navegação entre Telas com Jetpack Compose

> **Data do projeto:** Outubro de 2025

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Open Source](https://badges.frapsoft.com/os/v1/open-source.svg?v=103)](https://github.com/)
[![Last Updated](https://img.shields.io/badge/last%20update-2025--10--05-success)]()

> **Atenção:** Este projeto é um exemplo didático, criado para fins de estudo e demonstração. Não é recomendado para uso em produção sem as devidas adaptações e revisões de segurança, arquitetura e boas práticas.

Este projeto demonstra como implementar a navegação entre múltiplas telas em um aplicativo Android utilizando **Jetpack Compose** e **Navigation Compose**.

## ✨ Funcionalidades
- Tela de **Login**
- Tela de **Menu** com navegação para outras telas
- Tela de **Perfil** (recebe parâmetros via rota)
- Tela de **Pedidos** (recebe parâmetros via query)
- Navegação controlada por **NavController**
- Layouts modernos e responsivos com Compose

## 🚀 Tecnologias Utilizadas
- [Kotlin](https://kotlinlang.org/)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Navigation Compose](https://developer.android.com/jetpack/compose/navigation)

## 📱 Estrutura das Telas
- **LoginScreen**: Tela inicial, com botão para acessar o menu.
- **MenuScreen**: Exibe opções para navegar para Perfil, Pedidos ou sair.
- **PerfilScreen**: Mostra informações do perfil, recebendo parâmetros pela rota.
- **PedidosScreen**: Exibe pedidos, recebendo parâmetros via query string.

## 🧭 Como funciona a navegação?
- O **NavController** é criado na `MainActivity` e passado para cada tela.
- As rotas são definidas no `NavHost`.
- Cada tela pode navegar para outra usando o `navController.navigate()`.
- Parâmetros podem ser passados via rota (ex: `perfil/{nome}/{idade}`) ou query string (ex: `pedidos?cliente=XPTO`).

## 🔄 Fluxo de Navegação entre Telas

A navegação entre as telas do aplicativo segue o fluxo ilustrado abaixo:

![Fluxo de navegação entre telas](docs/navigation-flow.png)

- **Login → Menu:** Ao clicar em "ENTRAR" na tela de Login, o usuário é direcionado para a tela de Menu.
- **Menu → Perfil:** O botão "Perfil" leva o usuário para a tela de Perfil.
- **Menu → Pedidos:** O botão "Pedidos" leva o usuário para a tela de Pedidos.
- **Menu → Sair:** O botão "Sair" retorna o usuário para a tela de Login.
- **Perfil → Menu:** O botão "Voltar" na tela de Perfil retorna para o Menu.
- **Pedidos → Menu:** O botão "Voltar" na tela de Pedidos retorna para o Menu.

As setas amarelas representam a navegação principal (avanço), enquanto as setas roxas representam o retorno para a tela anterior.

Esse fluxo garante uma navegação simples e intuitiva, permitindo que o usuário acesse facilmente as principais funcionalidades do app e retorne ao menu sempre que desejar.

> **Observação:** Para visualizar a imagem do fluxo, salve o diagrama acima como `docs/navigation-flow.png` no seu projeto.

## 📂 Estrutura de Pastas
```
app/
 └── src/
      └── main/
           └── java/
                └── carreiras/com/github/navigation_between_screens/
                     ├── MainActivity.kt
                     └── screens/
                          ├── LoginScreen.kt
                          ├── MenuScreen.kt
                          ├── PerfilScreen.kt
                          └── PedidosScreen.kt
```

## 🛠️ Como rodar o projeto
1. Clone este repositório
2. Abra no Android Studio
3. Execute em um emulador ou dispositivo físico

## 💡 Exemplos de Navegação
- Do Login para o Menu:
  ```kotlin
  navController.navigate("menu")
  ```
- Do Menu para Perfil (com parâmetros):
  ```kotlin
  navController.navigate("perfil/Fulano/27")
  ```
- Do Menu para Pedidos (com query):
  ```kotlin
  navController.navigate("pedidos?cliente=Cliente XPTO")
  ```

## 📋 Observações
- O projeto utiliza o padrão de passar o `modifier` com o `innerPadding` do Scaffold para garantir que o conteúdo não fique sobreposto por barras do sistema.
- O código está organizado para facilitar a expansão e manutenção.

## 🧭 Como funciona o NavController?

O `NavController` é o componente central do Navigation Compose e é responsável por gerenciar toda a navegação entre as telas (composables) do app. Ele funciona como um "guia" que sabe qual tela está sendo exibida e para qual tela o app deve navegar a seguir.

### Como o NavController é utilizado neste projeto?

1. **Criação do NavController:**
   - O `NavController` é criado na `MainActivity` usando a função `rememberNavController()`. Isso garante que o controller seja preservado durante recomposições.
   ```kotlin
   val navController = rememberNavController()
   ```

2. **Definição das rotas no NavHost:**
   - O `NavHost` recebe o `navController` e define as rotas (nomes das telas) e qual composable será exibido para cada rota.
   ```kotlin
   NavHost(navController = navController, startDestination = "login") {
       composable("login") { LoginScreen(navController = navController) }
       composable("menu") { MenuScreen(navController = navController) }
       composable("perfil/{nome}") { backStackEntry ->
           val nome = backStackEntry.arguments?.getString("nome") ?: "Usuário"
           PerfilScreen(navController = navController, nome = nome)
       }
       composable("pedidos") { PedidosScreen(navController = navController) }
   }
   ```

3. **Navegação entre telas:**
   - Cada tela recebe o `navController` como parâmetro. Assim, ao clicar em um botão, a tela pode chamar:
   ```kotlin
   navController.navigate("menu")
   navController.navigate("perfil/Fulano")
   navController.popBackStack() // Para voltar
   ```
   - Isso faz com que o NavController troque a tela exibida, de acordo com a rota informada.

4. **Passagem de parâmetros:**
   - Parâmetros podem ser passados na rota (ex: `perfil/{nome}`) e recuperados na tela de destino.

5. **Controle da pilha de navegação:**
   - O NavController mantém uma pilha de telas visitadas (stack), semelhante a uma pilha de pratos: a tela mais recente fica no topo e é a visível ao usuário (como ilustrado na imagem abaixo).
   - Quando você navega para uma nova tela usando `navController.navigate("rota")`, essa tela é empilhada sobre as anteriores.
   - O método `popBackStack()` remove a tela do topo da pilha, voltando para a tela anterior, exatamente como o botão "Voltar" do Android.
   - Exemplo visual:

     | Composable visível ao usuário |
     |------------------------------|
     | MenuScreen                   |
     | LoginScreen                  |
     | (base da pilha)              |

     Se você navegar para PerfilScreen:

     | Composable visível ao usuário |
     |------------------------------|
     | PerfilScreen                 |
     | MenuScreen                   |
     | LoginScreen                  |
     | (base da pilha)              |

     Ao chamar `popBackStack()`, PerfilScreen é removida e MenuScreen volta a ser visível.
   - Esse mecanismo garante que o usuário possa avançar e voltar entre telas de forma previsível, mantendo o histórico de navegação.

### Vantagens do NavController
- Centraliza e organiza a navegação.
- Permite passagem de dados entre telas.
- Facilita o controle do fluxo de telas e o comportamento do botão de voltar.
- Integra-se facilmente com o Jetpack Compose.

> **Resumo:** O NavController é o "cérebro" da navegação no app. Ele sabe qual tela mostrar, como passar dados entre elas e como voltar para telas anteriores, tornando o fluxo do app previsível e fácil de entender.

---

## 🧩 Como funciona a passagem de parâmetros entre telas?

- Parâmetros podem ser passados diretamente na rota, como em:
  ```kotlin
  navController.navigate("perfil/Fulano/27")
  ```
- Na tela de destino, eles são recuperados via argumentos:
  ```kotlin
  val nome = backStackEntry.arguments?.getString("nome")
  val idade = backStackEntry.arguments?.getString("idade")?.toIntOrNull() ?: 0
  ```
- Para queries, como em PedidosScreen:
  ```kotlin
  navController.navigate("pedidos?cliente=Cliente XPTO")
  // Recuperação:
  val cliente = backStackEntry.arguments?.getString("cliente")
  ```

## 🛡️ Por que usar Scaffold e innerPadding?

O Scaffold é um componente que organiza a estrutura visual do app (barras, conteúdo, etc). O innerPadding garante que o conteúdo principal não fique escondido atrás dessas barras, tornando o layout seguro para diferentes dispositivos e tamanhos de tela. Sempre aplique o innerPadding ao modifier das telas para evitar sobreposição com barras do sistema.

## 🏗️ Exercícios sugeridos

- Adicione uma nova tela chamada "Sobre" e crie um botão no Menu para acessá-la.
- Modifique o Menu para exibir o nome do usuário recebido do Login.
- Experimente passar mais parâmetros entre as telas.
- Altere as cores das telas para personalizar o visual.
- Implemente uma navegação de "logout" que limpe a pilha de navegação.

## 🔗 Links úteis

- [Documentação Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Documentação Navigation Compose](https://developer.android.com/jetpack/compose/navigation)
- [Exemplo oficial de navegação](https://developer.android.com/jetpack/compose/navigation#navhost)
- [Material Design Components](https://m3.material.io/)

Feito com 💙 usando Jetpack Compose!
