# Dashboard Financeiro Moderno - Instruções

1. Instale Python 3.8+ no seu PC: https://www.python.org/downloads/
2. Abra o Prompt de Comando e navegue até esta pasta:
   cd C:\caminho\para\FinanceDashboard
3. Instale os pacotes necessários:
   pip install -r requirements.txt
4. Para testar no navegador:
   streamlit run app.py
5. Para gerar o .exe:
   pyinstaller --onefile --windowed app.py
6. O executável será gerado em: dist\app.exe
7. Abra app.exe para usar o dashboard diretamente.
