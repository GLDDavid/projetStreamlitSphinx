CheatSheet !
============

Installer et importer
---------------------

.. code-block:: python
    :linenos:
    
    pip install streamlit

    import streamlit as st

Ligne de commande
-----------------

.. code-block:: python
    :linenos:
    
    # Aide
    streamlit --help
    
    # Pour lancer l’application
    streamlit run your_script.py

    # Pour lancer l’application par défaut
    streamlit hello

    # Affiche toutes les informations de configuration
    streamlit config show

    # Efface les fichiers persistants du cache
    streamlit cache clear

    # Ouvre la documentation streamlit dans un navigateur
    streamlit docs

    # Pour connaître la version installée de streamlit
    streamlit --version

Commandes magiques
------------------

Les commandes magiques sont une fonctionnalité de Streamlit qui vous permet d'écrire presque n'importe quoi (markdown, données, graphiques) sans avoir à taper une commande explicite. 

Placer simplement l'élément que vous souhaitez afficher sur sa propre ligne de code et il apparaîtra dans votre application.

.. code-block:: python
    :linenos:

    # Magic commands implicitly
    # call st.write().
    '_This_ is some **Markdown***'
    my_variable
    'dataframe:', my_data_frame

Afficher le texte
-----------------

.. code-block:: python
    :linenos:

    # Pour afficher un bloc de texte
    st.text('Fixed width text')

    # Pour afficher un texte façon markdown
    st.markdown('_Markdown_')

    # Affichez des expressions mathématiques
    st.latex(r''' e^{i\pi} + 1 = 0 ''')

    # Permet de passer des arguments
    st.write('Most objects') # df, err, func!
    st.write(['st', 'is <', 3])
    # Afficher le texte dans la mise en forme du titre
    st.title('My title')

    # Afficher le texte dans le format d'en-tête.
    st.header('My header')

    # Afficher le texte en format de sous-en-tête
    st.subheader('My sub')

    # Affichez un bloc de code avec une coloration syntaxique facultative
    st.code('for i in range(8): foo()')

Afficher les médias
-------------------

.. code-block:: python
    :linenos:

    # Afficher une image ou une liste d'images.
    st.image('./header.png')

    # Affichez un lecteur audio.
    st.audio(data)

    # Afficher un lecteur vidéo.
    st.video(data)

Ajouter des widgets à la barre latérale
---------------------------------------

.. code-block:: python
    :linenos:

    # Afficher un widget de bouton radio
    # dans la sidebar
    >>> a = st.sidebar.radio('Select one:', [1, 2])

    # Avec l’utilisation de ‘With’:
    >>> with st.sidebar:
    >>>   st.radio('Select one:', [1, 2])

Colonnes
--------

.. code-block:: python
    :linenos:

    # 2 colonnes égales:
    >>> col1, col2 = st.columns(2)
    >>> col1.write("This is column 1")
    >>> col2.write("This is column 2")


    # 3 colonnes différentes:
    >>> col1, col2, col3 = st.columns([3, 1, 1])
    # col1 is larger.


    # En utilisant le ‘With’:
    >>> with col1:
    >>>   st.radio('Select one:', [1, 2])

Flux de contrôle
----------------

.. code-block:: python
    :linenos:

    # Arreter l’execution  immédiatement:
    st.stop()

    # Relance le script immédiatement:
    st.experimental_rerun()


    # Groupe multiple de widgets:
    >>> with st.form(key='my_form'):
    >>>   username = st.text_input('Username')
    >>>   password = st.text_input('Password')
    >>>   st.form_submit_button('Login')

Afficher des widgets interactifs
--------------------------------

.. code-block:: python
    :linenos:

    # Afficher un widget bouton
    st.button('Click me')

    # Afficher un widget de case à cocher
    st.checkbox('I agree')

    # Afficher un widget de bouton radio.
    st.radio('Pick one', ['cats', 'dogs'])

    # Afficher un widget de selection
    st.selectbox('Pick one', ['cats', 'dogs'])

    # Afficher un widget de selection multiple
    st.multiselect('Buy', ['milk', 'apples', 'potatoes'])

    # Afficher un curseur
    st.slider('Pick a number', 0, 100)

    # Afficher un widget en curseur pour sélectionner un élément dans une liste
    st.select_slider('Pick a size', ['S', 'M', 'L'])

    # Afficher un widget de saisie de texte en une seule ligne
    st.text_input('First name')

    # Afficher un widget de saisie numérique
    st.number_input('Pick a number', 0, 10)

    # Afficher un widget de saisie de texte multiligne
    st.text_area('Text to translate')

    # Afficher un widget de saisie de date
    st.date_input('Your birthday')

    # Afficher un widget d’entrée de temps
    st.time_input('Meeting time')

    # Afficher un widget de téléchargement de fichiers
    st.file_uploader('Upload a CSV')

    # Afficher un widget qui renvoie les images de la webcam de l’utilisateur
    st.camera_input('Take a picture')

    # Afficher un widget bouton de téléchargement
    st.download_button('Download file', data)

    # Afficher un widget de sélection de couleur
    st.color_picker('Pick a color')

    #Utiliser les valeurs renvoyées par les widgets dans la variable:
    >>> for i in range(int(st.number_input('Num:'))):
    >>>   foo()
    >>> if st.sidebar.selectbox('I:',['f']) == 'f':
    >>>   b()
    >>> my_slider_val = st.slider('Quinn Mallory', 1, 88)
    >>> st.write(slider_val)

    #Désactiver les widgets pour supprimer l'interactivité:
    >>> st.slider('Pick a number', 0, 100, disabled=True)

Muter les données
-----------------

.. code-block:: python
    :linenos:

    # Ajouter des lignes à un dataframe.
    >>> element = st.dataframe(df1)
    >>> element.add_rows(df2)

    # Ajouter des lignes à un graphique.
    >>> element = st.line_chart(df1)
    >>> element.add_rows(df2)

Code d'affichage
----------------

.. code-block:: python
    :linenos:

    >>> with st.echo():
    >>>   st.write('Code will be executed and printed')

Espaces réservés, aide et options
---------------------------------

.. code-block:: python
    :linenos:

    # Remplacer n’importe quel élément.
    >>> element = st.empty()
    >>> element.line_chart(...)
    >>> element.text_input(...)  # Remplace le précédent.

    # Insertion dans le désordre
    >>> elements = st.container()
    >>> elements.line_chart(...)
    >>> st.write("Hello")
    >>> elements.text_input(...)  # Apparaît au-dessus de "Hello".

    st.help(pandas.DataFrame)
    st.get_option(key)
    st.set_option(key, value)
    st.set_page_config(layout='wide')
    st.experimental_show(objects)
    st.experimental_get_query_params()
    st.experimental_set_query_params(**params)

Optimiser les performances
--------------------------

Mise en cache héritée
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python
    :linenos:

    >>> @st.cache
    ... def foo(bar):
    ...   # Do something expensive in here...
    ...   return data
    >>> # Exécute foo
    >>> d1 = foo(ref1)
    >>> # N’exécute pas foo
    >>> # Renvoie l’élément mis en cache par référence, d1 == d2
    >>> d2 = foo(ref1)
    >>> # Arg différent, donc la fonction foo s’exécute
    >>> d3 = foo(ref2)

Objets de données en cache
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python
    :linenos:

    # Par exemple : Calcul de dataframe, stockage des données téléchargées, …
    >>> @st.experimental_memo
    ... def foo(bar):
    ...   # Faire quelque chose et renvoyer des données
    ...   return data
    # Execute foo
    >>> d1 = foo(ref1)
    # N’exécute pas foo
    # Renvoie l’élément mis en cache par valeur, d1 == d2
    >>> d2 = foo(ref1)
    # arg différent, donc la fonction foo s’exécute
    >>> d3 = foo(ref2)
    # Effacer toutes les entrées en cache pour cette fonction 
    >>> foo.clear()
    # Effacer les valeurs de *toutes* les fonctions mémorisées   
    >>> st.experimental_memo.clear()

Mettre en cache des objets autres que des données
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python
    :linenos:

    # Exemple : Session TensorFlow, connexion à la base de données, ...
    >>> @st.experimental_singleton
    ... def foo(bar):
    ...   # Créer et renvoyer un objet
    ...   return session
    # Exécute foo
    >>> s1 = foo(ref1)
    # N’exécute pas foo
    # Renvoie l’élément mis en cache par référence, d1 == d2
    >>> s2 = foo(ref1)
    # arg différent, donc la fonction foo s’exécute
    >>> s3 = foo(ref2)
    # Effacer toutes les entrées en cache pour cette fonction 
    >>> foo.clear()
    # Effacer toutes les singleton caches
    >>> st.experimental_singleton.clear()

Afficher la progression et l'état
---------------------------------

.. code-block:: python
    :linenos:

    >>> with st.spinner(text='In progress'):
    >>>   time.sleep(5)
    >>>   st.success('Done')

    # Afficher la barre de progression
    st.progress(progress_variable_1_to_100)

    # Dessiner des ballons de fête
    st.balloons()

    # Dessiner des chutes de neiges festives
    st.snow()

    # Afficher le message d’erreur
    st.error('Error message')

    # Afficher le message d’avertissement
    st.warning('Warning message')

    # Afficher un message d’information
    st.info('Info message')

    # Afficher un message de réussite
    st.success('Success message')

    # Afficher une exception
    st.exception(e)
