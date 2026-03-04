# ARCH — Preencha após refatoração

## Estrutura final (cole a árvore de pastas)

lib/
  core/
    utils/

  features/
    todos/
      data/
        datasources/
          todo_remote_datasource.dart
          todo_local_datasource.dart
        models/
          todo_model.dart
        repositories/
          todo_repository_impl.dart

      domain/
        entities/
          todo.dart
        repositories/
          todo_repository.dart

      presentation/
        screens/
          todos_page.dart
        viewmodels/
          todo_viewmodel.dart
        widgets/
          add_todo_dialog.dart

  main.dart

## Fluxo de dependências
UI -> ViewModel -> Repository -> (RemoteDataSource, LocalDataSource)

Fluxo detalhado:

TodosPage (UI)
  ↓
TodoViewModel
  ↓
TodoRepository (interface - domain)
  ↓
TodoRepositoryImpl (data)
  ↓
TodoRemoteDataSource / TodoLocalDataSource

## Decisões
- Onde ficou a validação?
A validação de regras de negócio ficou na camada de Domain (ou no ViewModel quando relacionada a estado de UI). 
Exemplo: impedir adicionar uma tarefa com título vazio.

- Onde ficou o parsing JSON?
O parsing de JSON ficou na camada Data, dentro do TodoModel, utilizando factory constructors como fromJson().
A camada Domain não conhece JSON.

- Como você tratou erros?
Erros de acesso a dados (HTTP, storage) são tratados na camada Data.
O Repository centraliza o tratamento e pode lançar exceções ou retornar estados controlados para o ViewModel.
O ViewModel converte erros em estados para a UI exibir mensagens ao usuário.
