import streamlit as st
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
from datetime import datetime, timedelta
import os

# Configuration de la page
st.set_page_config(
    page_title="ID SMART PREDICTms-python.pythonOR V2.1",
    page_icon="⚽",
    layout="wide",
    initial_sidebar_state="expanded"
)

# CSS personnalisé pour le style français
st.markdown("""
<style>
    .main-header {
        text-align: center;
        color: #1f4e79;
        font-size: 2.5rem;
        font-weight: bold;
        margin-bottom: 1rem;
    }
    .subtitle {
        text-align: center;
        color: #666;
        font-size: 1.2rem;
        margin-bottom: 2rem;
    }
    .metric-card {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        padding: 1rem;
        border-radius: 0.5rem;
        margin: 0.5rem 0;
    }
</style>
""", unsafe_allow_html=True)

def main():
    # En-tête principal
    st.markdown('<h1 class="main-header">⚽ ID SMART PREDICTOR V2.1</h1>', unsafe_allow_html=True)
    st.markdown('<p class="subtitle">Analyses et prédictions football intelligentes</p>', unsafe_allow_html=True)
    
    # Sidebar pour la navigation
    st.sidebar.title("🏆 Navigation")
    st.sidebar.markdown("---")
    
    # Informations sur l'application
    with st.sidebar.expander("ℹ️ À propos"):
        st.write("""
        **ID SMART PREDICTOR V2.1** utilise l'intelligence artificielle pour:
        - Prédire les résultats de matchs
        - Analyser les performances des équipes
        - Comparer les statistiques des joueurs
        - Suivre les classements en temps réel
        """)
    
    # Dashboard principal
    st.header("📈 Tableau de Bord Principal")
    
    # Métriques rapides
    col1, col2, col3, col4 = st.columns(4)
    
    with col1:
        st.metric(
            label="🎯 Précision des Prédictions",
            value="87.3%",
            delta="2.1%"
        )
    
    with col2:
        st.metric(
            label="📊 Matchs Analysés",
            value="2,847",
            delta="156"
        )
    
    with col3:
        st.metric(
            label="⚽ Équipes Suivies",
            value="120",
            delta="5"
        )
    
    with col4:
        st.metric(
            label="👤 Joueurs Analysés",
            value="3,421",
            delta="89"
        )
    
    st.markdown("---")
    
    # Section des prédictions du jour
    st.subheader("🔮 Prédictions du Jour")
    
    # Données d'exemple pour les prédictions du jour
    predictions_data = {
        'Équipe Domicile': ['Paris SG', 'Marseille', 'Lyon', 'Monaco'],
        'Équipe Extérieur': ['Lille', 'Rennes', 'Nice', 'Lens'],
        'Prédiction': ['Victoire PSG', 'Match Nul', 'Victoire Lyon', 'Victoire Monaco'],
        'Probabilité': [78.5, 45.2, 67.8, 82.1],
        'Cote Estimée': [1.35, 2.85, 1.67, 1.28]
    }
    
    predictions_df = pd.DataFrame(predictions_data)
    
    # Affichage du tableau des prédictions
    for index, row in predictions_df.iterrows():
        col1, col2, col3, col4, col5 = st.columns([3, 1, 3, 2, 2])
        
        with col1:
            st.write(f"**{row['Équipe Domicile']}**")
        
        with col2:
            st.write("🆚")
        
        with col3:
            st.write(f"**{row['Équipe Extérieur']}**")
        
        with col4:
            confidence_color = "green" if row['Probabilité'] > 70 else "orange" if row['Probabilité'] > 50 else "red"
            st.markdown(f"<span style='color: {confidence_color}'>{row['Prédiction']}</span>", unsafe_allow_html=True)
            st.write(f"Confiance: {row['Probabilité']}%")
        
        with col5:
            st.write(f"Cote: {row['Cote Estimée']}")
    
    st.markdown("---")
    
    # Graphiques de performance
    col1, col2 = st.columns(2)
    
    with col1:
        st.subheader("📊 Précision des Prédictions par Mois")
        months = ['Jan', 'Fév', 'Mar', 'Avr', 'Mai', 'Jun']
        accuracy = [82.1, 84.5, 86.2, 85.8, 87.3, 88.1]
        
        fig_accuracy = px.line(
            x=months, 
            y=accuracy,
            title="Évolution de la Précision",
            labels={'x': 'Mois', 'y': 'Précision (%)'}
        )
        fig_accuracy.update_traces(line_color='#667eea', line_width=3)
        st.plotly_chart(fig_accuracy, use_container_width=True)
    
    with col2:
        st.subheader("🎯 Répartition des Prédictions")
        labels = ['Victoires Domicile', 'Victoires Extérieur', 'Matchs Nuls']
        values = [45, 35, 20]
        
        fig_pie = px.pie(
            values=values, 
            names=labels,
            title="Types de Prédictions"
        )
        fig_pie.update_traces(
            textposition='inside', 
            textinfo='percent+label',
            marker=dict(colors=['#667eea', '#764ba2', '#f093fb'])
        )
        st.plotly_chart(fig_pie, use_container_width=True)
    
    # Section des actualités
    st.markdown("---")
    st.subheader("📰 Actualités Football")
    
    news_col1, news_col2, news_col3 = st.columns(3)
    
    with news_col1:
        with st.container():
            st.markdown("**🔥 Match à Suivre**")
            st.write("PSG vs Marseille - Classique attendu ce weekend")
            st.write("Probabilité PSG: 68%")
    
    with news_col2:
        with st.container():
            st.markdown("**⭐ Joueur en Forme**")
            st.write("Kylian Mbappé - 12 buts en 8 matchs")
            st.write("Performance: +23% vs moyenne")
    
    with news_col3:
        with st.container():
            st.markdown("**📈 Tendance**")
            st.write("Lyon retrouve sa forme avec 4 victoires consécutives")
            st.write("Cote de confiance: 85%")
    
    # Footer
    st.markdown("---")
    st.markdown(
        """
        <div style='text-align: center; color: #666; padding: 1rem;'>
        ID SMART PREDICTOR V2.1 - Analyse football intelligente<br>
        Dernière mise à jour: {date}
        </div>
        """.format(date=datetime.now().strftime("%d/%m/%Y %H:%M")),
        unsafe_allow_html=True
    )

if __name__ == "__main__":
    main()
