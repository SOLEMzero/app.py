import streamlit as st
import pandas as pd
import os

st.set_page_config(page_title="Dashboard Financeiro Moderno", layout="wide", page_icon="💸")

st.markdown("<h1 style='text-align:center; color:#4B0082;'>💸 Dashboard Financeiro Moderno</h1>", unsafe_allow_html=True)

# Arquivo CSV para salvar dados
ARQUIVO = "dados.csv"

# Carregar dados
if os.path.exists(ARQUIVO):
    df = pd.read_csv(ARQUIVO)
else:
    df = pd.DataFrame(columns=["Tipo", "Categoria", "Valor"])

# ===== Sidebar =====
st.sidebar.header("➕ Nova Transação")
tipo = st.sidebar.selectbox("Tipo", ["Receita", "Despesa"])
categoria = st.sidebar.text_input("Categoria")
valor = st.sidebar.number_input("Valor", min_value=0.0, format="%.2f")

if st.sidebar.button("Adicionar"):
    novo = pd.DataFrame([[tipo, categoria, valor]], columns=["Tipo", "Categoria", "Valor"])
    df = pd.concat([df, novo], ignore_index=True)
    df.to_csv(ARQUIVO, index=False)
    st.success("✅ Transação adicionada!")

# ===== KPIs =====
st.markdown("## 📊 Resumo Financeiro")
col1, col2, col3 = st.columns(3)

receitas = df[df["Tipo"]=="Receita"]["Valor"].sum()
despesas = df[df["Tipo"]=="Despesa"]["Valor"].sum()
saldo = receitas - despesas

col1.metric("💰 Receitas", f"R$ {receitas:,.2f}", delta=f"R$ {receitas - despesas:,.2f}")
col2.metric("💸 Despesas", f"R$ {despesas:,.2f}")
col3.metric("📊 Saldo", f"R$ {saldo:,.2f}")

# ===== Gráfico combinado =====
st.markdown("## 📈 Gráfico de Despesas x Receitas")
if not df.empty:
    resumo = df.groupby(["Tipo","Categoria"])["Valor"].sum().unstack(fill_value=0)
    st.bar_chart(resumo)  # barras empilhadas por categoria
else:
    st.info("Adicione transações para visualizar o gráfico.")

# ===== Histórico =====
st.markdown("## 📋 Histórico de Transações")
st.dataframe(df, use_container_width=True)

# ===== Limpar dados =====
if st.button("🗑️ Limpar Dados"):
    df = pd.DataFrame(columns=["Tipo","Categoria","Valor"])
    df.to_csv(ARQUIVO, index=False)
    st.warning("Todos os dados foram apagados!")

# ===== Estilo adicional =====
st.markdown("""
<style>
[data-testid="stMetricDelta"] svg {
    display: none;  /* remove setinhas automáticas para visual mais limpo */
}
</style>
""", unsafe_allow_html=True)
