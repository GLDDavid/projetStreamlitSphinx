Demonstration
==================

Effet WAOUH Garanti
--------------------


Vous l'avez compris, STREAMLIT est génial.
Nous allons ici faire une démonstration de Streamlit.

Partons à la découverte de ce framework.

Pour commencer nous allons faire quelques lignes de commande dans le terminal.

.. code-block:: python
    :linenos:

    cd Documents
	mkdir demoStreamlit
	cd demoStreamlit
		MAC			                Windows
	touch main.py 			ou     		type nul > main.py
	python -m venv venv
    cd..
		MAC			                Windows
	ls demoStreamlit	    ou     		dir demoStreamlit
	exit


La commande dir ou ls nous permet de voir que nous avons bien la présence d'un fichier main.py et d'un dossier venv.

Allons maintenant dans notre éditeur de code VSCode. On ouvre le dossier demoStreamlit et avec **ctrl ù** on ouvre un terminal.
Pas un powershell mais bien un command prompt.

.. code-block:: python
    :linenos:

	cd venv
	cd Scripts
	activate
	cd../..
	pip install streamlit
	streamlit hello


Et voilà, vous venez de créer une application web de machine learning.
Elle est bien évidement Responsive( inspecter et choisir différente taille d’écran).

Dans la sidebar sur la gauche vous avez la possibilité de choisir différentes Démo.

On va maintenant faire un **ctrl c** dans le terminal de VSCode (avant de fermer la page)

.. code-block:: python
    :linenos:

	streamlit run main.py

Cette commande va ouvrir une fenêtre dans notre navigateur.

Sur l’app streamlit, aller sur le menu burger à droite. Dans settings ou Réglages, cocher la case Run on save ou exécuter à la sauvegarde afin de relancer notre application à chaque sauvegarde dans VSCode.
C'est quand même plus sympa.

Maintenant dans main.py

Nous allons écrire notre fameux HELLO WORLD !
C'est truc à faire quand on est un pro du web.
Il va quand même falloir importer streamlit


.. code-block:: python
    :linenos:

	import streamlit as st
	# 1
	st.write('Hello, world !')


Petit raccourci clavier **ctrl s** pour sauvegarder dans VSCode


.. figure::  ./_static/images/unicorn-magic.gif
    :alt: gif effet wahou
    :align: center


Nous allons avec un slider faire une opération mathématique comme par exemple le carré de x

.. code-block:: python
    :linenos:

	# 2
	x = st.slider('x')
	st.write(x, 'au carré est ', x*x)


On va se servir de pandas pour cette petite démo avec read_csv, je vais récupérer un dataframe, le filtrer grace à un selectbox
et afficher en dessous mon tableau de données filtrées

.. code-block:: python
    :linenos:

	# 3
	import pandas as pd

	read_and_cache_csv = st.cache(pd.read_csv)
	BUCKET = "https://streamlit-self-driving.s3-us-west-2.amazonaws.com/"
	data = read_and_cache_csv(BUCKET + "labels.csv.gz", nrows=1000)
	desired_label =st.selectbox('Filter to :', ['car', 'truck'])
	st.write(data[data.label == desired_label])

Maintenant on va aller sur <https://github.com/streamlit> et sur **demo-uber-nyc-pickups**.
On va copier le code streamlit_app.py à partir de la ligne 18 jusqu’à la fin et il faut télécharger le fichier uber-raw-data-sep14.csv.gz et le mettre dans demoStreamlit

On peut également supprimer tout ce que l’on a fait avant dans main.py.
Cependant il faut garder :

.. code-block:: python
    :linenos:

	import streamlit as st



Un petit peu de code :

On peut faire un titre

.. code-block:: python
    :linenos:

   	st.title("Mon Titre")

On peut aussi juste écrire du texte

.. code-block:: python
    :linenos:

	st.write("bla bla bla mon texte")

Il est également possible de faire du markdown

.. code-block:: python
    :linenos:

	st.write("# Mon Titre")

Il est possible par exemple d'utiliser une balise markdown à la place de write pour écrire un bloc de texte et gérer des espaces avec des #

.. code-block:: python
    :linenos:

	with row1_2:
    st.write(
        """
    ###
    Examiner comment les ramassages Uber varient au fil du temps à New York et dans ses principaux aéroports régionaux.
    En faisant glisser le curseur sur la gauche, vous pouvez afficher différentes tranches de temps et explorer différentes tendances en matière de transport.
    """
    )
    st.markdown(
        """
    #
    Examiner comment les ramassages Uber varient au fil du temps à New York et dans ses principaux aéroports régionaux.
    En faisant glisser le curseur sur la gauche, vous pouvez afficher différentes tranches de temps et explorer différentes tendances en matière de transport.
    """
    )

Plus on va mettre de # (limite : 6) moins l’espace sera important.
Bien faire attention à **"""**

On va maintenant tout effacer et on va faire une partie de cette appli.
Donc on supprime tout et on va écrire :

.. code-block:: python
    :linenos:

	import streamlit as st
	import pandas as pd
	
	DATE_TIME = "date/time"


	st.title("Uber Pickups in New York City")
	st.markdown(
		"""
		This is a demo of streamlit
		"""
	)
	@st.cache(persist=True)
	def load_data(nrows):
		data = pd.read_csv("uber-raw-data-sep14.csv.gz", nrows=nrows)
		lowercase = lambda x : str(x). lower()
		data.rename(lowercase, axis="columns", inplace=True)
		data[DATE_TIME] = pd.to_datetime(data[DATE_TIME])
		return data

	data = load_data(100000)


.. code-block:: python
    :linenos:

	st.write("il est possible de voir le dataset que l'on a récupérer avec 'data', ici on aura que 100000 lignes")
	data

il est possible de filtrer le dataframe mais avant on doit faire une modif sur le cache

.. code-block:: python
    :linenos:

	#@st.cache(persist=True)
	@st.cache()


.. code-block:: python
    :linenos:

	st.write("il est possible de filtrer. Ici par exemple on veut seulement les heures qui correspondent à 12")
	hour = 12
	data = data[data[DATE_TIME].dt.hour == hour]
	data


On peut indiquer l’heure au dessus du dataframe

.. code-block:: python
    :linenos:

	hour = 12
	data = data[data[DATE_TIME].dt.hour == hour]
	'## Filtre sélectionner %sh' % hour, data

Et si on faisait une petite carte avec st.map

.. code-block:: python
    :linenos:

	'## GEO Data à %sh'%hour
	st.map(data)

Bien évidemment tout est intéractif. C'est à dire que si je change l'heure dans mon code, mon dataframe va changer mais aussi la carte

Et si on faisais un slider pour sélectionner une heure ???

.. code-block:: python
    :linenos:

	#hour = 10
	hour = st.slider('heure', 0, 23, 10)

OK alors plutôt que d'avoir le slider juste au dessus de mon dataframe, si je faisais une sidebar (ouh la la, trop difficile...)

.. code-block:: python
    :linenos:

	#hour = 10
	#hour = st.slider('heure', 0, 23, 10)
	hour = st.sidebar.slider('heure', 0, 23, 10)

Ah et bien NON en fait. C'est trop génial Streamlit.

On passe à l'application ??

