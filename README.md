# Minhas Finanças

Aplicativo de controle financeiro pessoal feito em Python com Flet. Ajuda a acompanhar entradas, gastos, cartões de crédito, faturas, parcelamentos e metas de economia, com login protegido por verificação em duas etapas e trava por biometria.

## Funcionalidades

* Resumo do mês com a régua do mês: quanto entrou, quanto foi para gastos, faturas e metas, e quanto sobra.
* Extrato dos lançamentos agrupado por dia, com filtros por forma de pagamento (Pix, débito, dinheiro e crédito).
* Cartões de crédito com fatura do mês, limite usado e disponível, compras parceladas e pagamento parcial da fatura.
* Simulação de compras e pagamentos antes de fazer, mostrando o impacto no limite e no saldo de cada mês.
* Página de entradas com comparação com o mês anterior e gráfico dos últimos 6 meses.
* Metas para juntar dinheiro, com cálculo de quanto guardar por mês até o prazo.
* Layout responsivo: barra inferior no celular e menu lateral no computador.
* Modo demonstração com dados de exemplo, sem precisar de conta.

## Tecnologias

* Python 3.12 ou mais recente
* Flet 1.0.3 (interface)
* Supabase (banco de dados PostgreSQL, autenticação e verificação em duas etapas)
* Windows Hello e flet-local-auth (biometria do aparelho)

## Segurança

* Login com e-mail e senha, com o cadastro de novas contas bloqueado.
* Verificação em duas etapas com app autenticador (TOTP). O site e o celular guardam o mesmo segredo, entregue uma única vez pelo QR code, e cada um calcula o código de 6 dígitos a partir desse segredo e da hora atual. O código muda a cada 30 segundos.
* Regras de segurança no próprio banco (Row Level Security). Cada pessoa só enxerga os próprios dados, e quem tem o autenticador ativo só recebe os dados depois de informar o código.
* Trava por biometria do aparelho. O app pergunta ao sistema se é a dona do aparelho e recebe apenas sim ou não. O rosto e a digital nunca saem do aparelho e não são guardados pelo app.
* Validação de e-mail e do código com expressões regulares (REGEX) antes de enviar ao servidor.

## Estrutura do projeto

* app_flet/main.py: ponto de entrada do aplicativo
* app_flet/financas_app/app.py: telas, login, formulários e trava
* app_flet/financas_app/regras.py: cálculos de faturas, parcelas, limite, saldo e metas
* app_flet/financas_app/dados.py: comunicação com o Supabase e modo demonstração
* app_flet/financas_app/biometria.py: Windows Hello e biometria do celular
* app_flet/financas_app/ui.py: cores, fontes e componentes visuais
* app_flet/testes: testes automáticos das regras
* supabase/schema.sql: criação das tabelas e das regras de segurança do banco

## Como rodar

Na pasta app_flet:

    python -m venv .venv
    .venv\Scripts\pip install -r requirements.txt
    .venv\Scripts\python main.py

Para abrir no navegador em vez de uma janela:

    .venv\Scripts\python main.py --web

Na tela de entrada, use uma conta cadastrada ou clique em "Ver demonstração com dados de exemplo".

## Testes

As regras de cálculo são conferidas automaticamente com dados aleatórios:

    .venv\Scripts\python testes\test_regras_iguais_ao_site.py
