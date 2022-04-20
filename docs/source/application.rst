Application
============


Nous allons concevoir un dashboard afin de visualiser différents indicateurs concernant notre supermarché.


Création de l'environnement
------------------


Pour commencer il va falloir créer un dossier sur notre bureau que nous allons appeler :
	**streamlit_supermarket**
Dans ce dossier nous allons placer le csv ainsi que le requirements.txt que je vais vous envoyer.
Puis on va ouvrir ce dossier avec VSCode.

Dans VSCode, vous allez créer un fichier:
	**app.py**
Que l’on va laisser vide pour le moment.

Nous allons maintenant ouvrir le terminal dans VSCode avec le raccourci clavier **ctrl ù** et nous allons créer un environ virtuel.
 
.. code-block:: python
    :linenos:

    python -m venv venv
    cd venv
    cd Scripts
    activate
    cd../..

Après avoir notre environnement virtuel, nous allons pouvoir effectuer  nos imports. Pour cela vous pouvez récupérer le dossier file_streamlit dans lequel vous allez trouver les fichiers nécessaire pour cette application.

.. code-block:: python
    :linenos:

    pip install -r requirements.txt


Il est grand temps d'écrire notre premier Hello World.
Dans app.py nous allons écrire ce code.

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    import streamlit as st
    st.write('Hello world !')

Nous pouvons maintenant faire une petite vérification afin de voir si streamlit est bien importé.

.. code-block:: python
    :linenos:

    streamlit run app.py

A ce stade, une fenêtre doit s'ouvrir dans votre navigateur avec une application streamlit dans laquelle nous pouvons voir le formidable Hello world !

Sur l’app streamlit, aller sur le menu burger à droite. Dans settings ou Réglages, cocher la case Run on save ou exécuter à la sauvegarde afin de relancer notre application à chaque sauvegarde dans VSCode.
C'est quand même plus sympa.

Il est possible de fermer l'application avec l'instruction ctrl c dans le terminal

.. code-block:: python
    :linenos:

    ctrl c



Création de l'application
-----------------


Notre environnement est créé et il fonctionne bien. Nous allons maintenant pouvoir passer aux choses sérieuses.

**Import des dépendances et des données**

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    import streamlit as st
    import pandas as pd
    import plotly.express as px

    st.set_page_config( page_title="Dashboard des Ventes",
                        page_icon=':bar_chart:',
                        layout="wide")

    df = pd.read_csv('supermarkt_sales.csv', sep=';', decimal=',')

    st.dataframe(df)


On retourne dans le terminal et on relance l'application

.. code-block:: python
    :linenos:

    streamlit run app.py


**Sidebar**

Nous allons créer une sidebar afin de positionner différents widgets

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    #---- SIDEBAR ----
    st.sidebar.header("Mes Filtres:")
    city = st.sidebar.multiselect(
            "Selection de ville:",
            options=df['City'].unique(),
            default=df['City'].unique()
    )

On peut constater que sur notre application une sidebar a été créée et que nous avons les 3 villes unique présente dans notre dataframe

Vous allez maintenant faire la même chose avec les customer_type et le gender.
Pour voir le code, il suffit de cliquer sur le bouton en dessous.

.. toggle::
   
    .. code-block:: python
        :linenos:
        :caption: app.py
        :name: app-py

        customer_type = st.sidebar.multiselect(
            "Selection du type de client:",
            options=df['Customer_type'].unique(),
            default=df['Customer_type'].unique()
        )

        gender = st.sidebar.multiselect(
            "Selection du sexe:",
            options=df['Gender'].unique(),
            default=df['Gender'].unique()
        )


Avoir différents filtres c'est bien, mais encore faut-il que tout soit dynamique. Pour cela, il va falloir lier nos filtres à notre dataframe

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    df_selection = df.query(
        "City == @city & Customer_type == @customer_type & Gender == @gender"
    )

    st.dataframe(df_selection)
    # supprimer l'autre st.dataframe(df)


Un petit tour coté navigateur, afin de constater que tout fonctionne trés bien.
Dès que l'on modifie nos filtres, le dataframe change.


Passons maintenant aux **KPI**

Pour cela, nous allons avoir besoin de récupérer différentes valeurs et de les afficher dans des conteneurs.
Occupons nous déjà du titre.

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    #--- MAINPAGE -----
    st.title(':bar_chart: Dashboard des Ventes')
    st.markdown("##")


On peut maintenant créer nos indicateurs, à savoir le total des ventes, la note moyenne, la note star et la vente moyenne par transaction

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    # --- Top KPI ---
    total_ventes = int(df_selection["Total"].sum())
    note_moyenne = round(df_selection["Rating"].mean(), 1)
    note_star = ":star:" * int(round(note_moyenne, 0))
    vente_moyenne_par_transaction = round(df_selection['Total'].mean(), 2)


Pour afficher correctement le tout, on va créer des conteneurs grâce à columns

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    left_column, middle_column, right_column = st.columns(3)
    with left_column:
        st.subheader("Total des Ventes:")
        st.subheader(f"US $ {total_ventes:,}")
    with middle_column:
        st.subheader("Note Moyenne:")
        st.subheader(f"{note_moyenne} {note_star}")
    with right_column:
        st.subheader("vente moyenne par transaction")
        st.subheader(f"US $ {vente_moyenne_par_transaction}")


Et nous pouvons faire un joli trait en dessous pour bien séparer le tout

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    st.markdown("---")

A ce stade on peut voir qu’avec la sélection de différents filtres nos kpi changent correctement. C'est formidable !!!


**BAR CHART**

Nous allons maintenant créer 2 graphiques en barres 'bar_chart'

Nous commençons par récupérer les valeurs dans mon dataframe

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    # Ventes de produit [Bar Chart]
    ventes_par_gamme_de_produit = (
        df_selection.groupby(by=['Product line']).sum()[["Total"]].sort_values(by="Total")
    )


Puis nous allons concevoir le bar_chart et on va afficher tout ça avec plotly_chart

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    fig_ventes_produits = px.bar(
        ventes_par_gamme_de_produit,
        x="Total",
        y=ventes_par_gamme_de_produit.index,
        orientation="h",
        title="<b>Ventes par Gamme de Produits</b>",
        color_discrete_sequence=["#0083B8"] * len(ventes_par_gamme_de_produit),
        template="plotly_white",
    )
    st.plotly_chart(fig_ventes_produits)


Un peu de customisation pour le coté joli

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    fig_ventes_produits.update_layout(
        plot_bgcolor = "rgba(0,0,0,0)",
        xaxis=(dict(showgrid=False))
    )


Passons à notre deuxième graphiques.
Il va concerner les ventes en fonction des heures.
Pour cela Pandas est motre ami et nous pouvons créer une nouvelle colonne à partir de la colonne Time de mon dataframe

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    # ajouter la colonne hour au dataframe
    df['hour'] = pd.to_datetime(df['Time'], format="%H:%M").dt.hour


Il va falloir placer ce code juste aprés le read_csv. Cependant, pour éviter que notre code précédent ne se reproduise encore et encore
(Mais Ça continue encore et encore! C'est que le début d'accord, d'accord!
Merci Françis Cabrel)
Nous allons le placer dans une fonction, on va s'occuper du cache et on n'oublie d'appeller la fonction.

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    @st.cache
    def import_data():
        df = pd.read_csv('supermarkt_sales.csv', sep=';', decimal=',')
        # ajouter la colonne hour au dataframe
        df["hour"] = pd.to_datetime(df["Time"], format="%H:%M").dt.hour
        return df

    df = import_data()


Passons à notre graphique. On va pouvoir récupérer les valeurs de la colonne hour maintenant.

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    # Ventes par heures [BAR CHART]
    ventes_par_heures = df_selection.groupby(by=['hour']).sum()[['Total']]


Puis vous allez construire le prochain bar_chart

.. toggle::

    .. code-block:: python
        :linenos:
        :caption: app.py
        :name: app-py

        fig_vente_heures = px.bar(
            ventes_par_heures,
            x=ventes_par_heures.index,
            y='Total',
            title="<b>Ventes par heure</b>",
            color_discrete_sequence=["#0083B8"] * len(ventes_par_heures),
            template="plotly_white",
        )
        fig_vente_heures.update_layout(
            xaxis=dict(tickmode="linear"),
            plot_bgcolor="rgba(0,0,0,0)",
            yaxis=(dict(showgrid=False)),
        )
        st.plotly_chart(fig_vente_heures)


**Mazette! Que c'est beau**.
Mais la disposition de ces graph ne me plait pas. Ils sont l'un en dessous de l'autre et j'aimerai les avoir l'un à coté de l'autre.
Pour cela, on va déjà supprimer les 2 st.plotly_chart

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    st.plotly_chart(fig_ventes_produits)
    st.plotly_chart(fig_vente_heures)


Nous allons construire 2 colonnes et placer chaque graph dans une colonne

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    left_column, right_column = st.columns(2)


On place dans la colonne de gauche le fig_vente_heures

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    left_column.plotly_chart(fig_vente_heures, use_container_witdh=True)


Et à droite le fig_ventes_produits

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    right_column.plotly_chart(fig_ventes_produits, use_container_width=True)


Soit ces 3 lignes de code :

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    left_column, right_column = st.columns(2)
    left_column.plotly_chart(fig_vente_heures, use_container_witdh=True)
    right_column.plotly_chart(fig_ventes_produits, use_container_width=True)


Passons maintenant au **CSS**.
Mais non, rassurez vous, nous allons juste masquer certains éléments comme par exemple le menu burger, ou la mention fait avec streamlit dans le footer.

.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    # -- Ajout de style ---
    st_style = """
        <style>
        #MainMenu {visibility: hidden;}
        footer {visibility: hidden;}
        header {visibility: hidden;}
        </style>
    """
    st.markdown(st_style, unsafe_allow_html=True)

Enfin si on va en faire du **CSS** parce que j'aime beaucoup cela.
Je vais vous envoyer un dossier **.streamlit** dans lequel il y a un fichier **config.toml**
C'est dans ce fichier que l'on va pouvoir placer tout notre css, enfin les quelques lignes.

Il ne reste plus qu'à faire un ** ctrl c** dans le terminal et de relancer l'application afin de voir les changements de style.

.. code-block:: python
    :linenos:

    ctrl c
    streamlit run app.py

Et voilà, vous avez maintenant une application streamlit fonctionelle. Vous pouvez utiliser HEROKU ou streamlit cloud pour que n'importe qui dans le monde utilise votre application.
Simple et Efficace.

C'est COOL Streamlit.




.. code-block:: python
    :linenos:
    :caption: app.py
    :name: app-py

    import streamlit as st
    import pandas as pd
    import plotly.express as px

    st.set_page_config(page_title="Dashboard des Ventes",
                        page_icon=':bar_chart:',
                        layout="wide")

    @st.cache
    def import_data():
        df = pd.read_csv('supermarkt_sales.csv', sep=';', decimal=',')
        # ajouter la colonne hour au dataframe
        df["hour"] = pd.to_datetime(df["Time"], format="%H:%M").dt.hour
        return df

    df = import_data()


    #---- SIDEBAR ----
    st.sidebar.header("Mes Filtres:")
    city = st.sidebar.multiselect(
        "Selection de ville:",
        options=df['City'].unique(),
        default=df['City'].unique()
    )


    customer_type = st.sidebar.multiselect(
        "Selection du type de client:",
        options=df['Customer_type'].unique(),
        default=df['Customer_type'].unique()
    )

    gender = st.sidebar.multiselect(
        "Selection du sexe:",
        options=df['Gender'].unique(),
        default=df['Gender'].unique()
    )

    df_selection = df.query(
        "City == @city & Customer_type == @customer_type & Gender == @gender"
    )


    st.dataframe(df_selection)


    #--- MAINPAGE -----
    st.title(':bar_chart: Dashboard des Ventes')
    st.markdown("##")

    # --- Top KPI ---
    total_ventes = int(df_selection["Total"].sum())
    note_moyenne = round(df_selection["Rating"].mean(), 1)
    note_star = ":star:" * int(round(note_moyenne, 0))
    vente_moyenne_par_transaction = round(df_selection['Total'].mean(), 2)


    left_column, middle_column, right_column = st.columns(3)
    with left_column:
        st.subheader("Total des Ventes:")
        st.subheader(f"US $ {total_ventes:,}")
    with middle_column:
        st.subheader("Note Moyenne:")
        st.subheader(f"{note_moyenne} {note_star}")
    with right_column:
        st.subheader("vente moyenne par transaction")
        st.subheader(f"US $ {vente_moyenne_par_transaction}")

    st.markdown("---")


    # Ventes de produit [Bar Chart]
    ventes_par_gamme_de_produit = (
        df_selection.groupby(by=['Product line']).sum()[["Total"]].sort_values(by="Total")
    )
    fig_ventes_produits = px.bar(
        ventes_par_gamme_de_produit,
        x="Total",
        y=ventes_par_gamme_de_produit.index,
        orientation="h",
        title="<b>Ventes par Gamme de Produits</b>",
        color_discrete_sequence=["#0083B8"] * len(ventes_par_gamme_de_produit),
        template="plotly_white",
    )

    fig_ventes_produits.update_layout(
        plot_bgcolor = "rgba(0,0,0,0)",
        xaxis=(dict(showgrid=False))
    )


    # Ventes par heures [BAR CHART]
    ventes_par_heures = df_selection.groupby(by=['hour']).sum()[['Total']]
    fig_vente_heures = px.bar(
        ventes_par_heures,
        x=ventes_par_heures.index,
        y='Total',
        title="<b>Ventes par heure</b>",
        color_discrete_sequence=["#0083B8"] * len(ventes_par_heures),
        template="plotly_white",
    )
    fig_vente_heures.update_layout(
        xaxis=dict(tickmode="linear"),
        plot_bgcolor="rgba(0,0,0,0)",
        yaxis=(dict(showgrid=False)),
    )

    left_column, right_column = st.columns(2)
    left_column.plotly_chart(fig_vente_heures, use_container_witdh=True)
    right_column.plotly_chart(fig_ventes_produits, use_container_width=True)


    # -- Ajout de sytle ---
    st_style = """
        <style>
        #MainMenu {visibility: hidden;}
        footer {visibility: hidden;}
        header {visibility: hidden;}
        </style>
    """
    st.markdown(st_style, unsafe_allow_html=True)