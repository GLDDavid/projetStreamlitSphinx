Qu'est-ce que Streamlit ?
=========================

Principes
---------

Le moyen le plus rapide de créer et de partager des applications de données.

**Streamlit** vous permet de transformer des scripts de données en applications Web partageables en quelques minutes. Tout est Python, open-source et gratuit ! Et une fois que vous avez créé une application, vous pouvez utiliser la plateforme cloud ( streamlit cloud ) pour gérer et partager votre application !

Ce framework permet de créer des applications web qui pourront intégrer aisément des modèles de machine learning et des outils de visualisation de données.

Contrairement aux autres framework python (Dash,..) pour créer des applications autour de la données, **Streamlit** permet de créer des applications web sans écrire du code HTML.


Technologies
------------

.. figure::  ./_static/images/compatibility.PNG
    :alt: Compatibilité librairies
    :align: center

Il est également possible d'intégrer des éléments `HTML et Javascript <https://stackoverflow.com/questions/67977391/can-i-display-custom-javascript-in-streamlit-web-app>`_ avec Streamlit.

Liste des librairies compatibles à Streamlit : 

- `Bokeh <https://bokeh.org/>`_ : Bibliothèque de visualisation de données
- `Altair <https://altair-viz.github.io/>`_ : Visualisation déclarative en Python
- `Pytorch <https://pytorch.org/>`_ : Bibliothèque logicielle Python open source de machine learning
- `OpenCV <https://opencv.org/>`_ : Bibliothèque graphique open source de traitements d'image en temps réel
- `DECK.GL <https://deckgl.readthedocs.io/en/latest/>`_ : Analyse visuelle pour explorer de grands ensembles de données
- `Pandas <https://pandas.pydata.org/>`_ : Bibliothèque permettant la manipulation et l'analyse des données
- `Vega-Lite <https://vega.github.io/vega-lite/>`_ : Bibliothèque de haut niveau de graphiques interactifs
- `matplotlib <https://matplotlib.org/>`_ : Bibliothèque destinée à tracer et visualiser des données sous formes de graphiques
- `NumPy <https://numpy.org/>`_ : Bibliothèque destinée à manipuler des matrices ou tableaux multidimensionnels ainsi que des fonctions mathématiques opérant sur ces tableaux
- `scikit-learn <https://scikit-learn.org/stable/>`_ : Bibliothèque open source destinée à l'apprentissage automatique
- `TensorFlow <https://www.tensorflow.org/>`_ : Bibliothèque open source destinée à l'apprentissage automatique
- `plotly <https://plotly.com/python/>`_ : Bibliothèque graphique permettant de créer des graphiques interactifs 
- `Keras <https://keras.io/>`_ : Bibliothèque Python open source pour développer et évaluer des modèles de deep learning.

Contraintes
-----------

1. Localisation des données

Nous ne disposons pas encore de renseignements sur la sécurité de l'application cloud, elle est toutefois régulièrement mise à jour afin d’apporter de nouvelles fonctionnalités ainsi que des patchs de sécurité.
Les données de ce framework sont hébergées sur un serveur informatique (stockées dans un data center) aux Etats Unis et nous n’avons pas d'informations concernant sa conformité RGPD.


2. Sécurité des produits

    - Authentification unique
    Tous les accès et connexions à Streamlit sont effectués via un fournisseur SSO : GitHub, GSuite et de nombreuses autres plates-formes sont prises en charge. Nous pouvons travailler avec notre équipe informatique pour intégrer le fournisseur de votre choix. Streamlit ne stocke pas les mots de passe des clients.

    - Stockage des identifiants
    Streamlit chiffre les données sensibles des clients (par exemple, les secrets, les jetons d'authentification) au repos avec AES256, comme décrit dans la documentation de Google.

    - Autorisations et contrôle d'accès basé sur les rôles
    Les niveaux d'autorisation héritent des autorisations que nous avons attribuées dans GitHub. Les utilisateurs disposant d'un accès en écriture à un référentiel GitHub pour une application donnée pourront apporter des modifications dans la console d'administration Streamlit.

    Seuls les utilisateurs disposant d'un accès administrateur à un référentiel peuvent déployer et supprimer des applications .

3. Sécurité du réseau et des applications

    - Hébergement de données
    L'infrastructure physique est hébergée et gérée au sein de Google Cloud Platform (GCP) à l'aide de leurs centres de données sécurisés. Streamlit exploite de nombreuses fonctionnalités de sécurité, de confidentialité et de redondance intégrées à la plate-forme. GCP surveille en permanence ses centres de données pour détecter les risques et se soumet à des évaluations pour garantir la conformité aux normes de l'industrie. Les centres de données de GCP disposent de nombreuses accréditations, notamment ISO-27001, SOC 1 et SOC 2.

    - Cloud privé virtuel
    Tous les serveurs se trouvent dans un cloud privé virtuel (VPC) avec des pare-feu et des listes de contrôle d'accès réseau (ACL) pour permettre un accès externe à quelques points de terminaison API sélectionnés. Tous les autres services internes ne sont accessibles qu'au sein du VPC.

    - Chiffrement
    Toutes les applications Streamlit sont entièrement servies via HTTPS. Toutes les données envoyées vers ou depuis Streamlit sur l'Internet public sont cryptées en transit à l'aide d'un cryptage 256 bits. Les points de terminaison d'API et d'application sont TLS uniquement (v1.2). Streamlit n'utilise que des suites de chiffrement fortes et HTTP Secure Transport Security (HSTS) pour garantir que les navigateurs interagissent avec les applications Streamlit via HTTPS. Il chiffre également les données au repos à l'aide d'AES-256.

    - Autorisations et authentification
    L'accès aux données des clients est limité aux employés autorisés qui en ont besoin pour leur travail. Il gére un réseau d'entreprise à confiance zéro, de sorte qu'il n'y a pas de ressources d'entreprise ou de privilèges supplémentaires obtenus en étant sur le réseau interne de Streamlit. Il utilise une authentification unique à 2 facteurs (2FA) et applique des politiques de mots de passe solides pour garantir la protection de l'accès à tous les services liés au cloud.

    - Réponse aux incidents
    Il a un protocole interne pour gérer les événements de sécurité qui comprend des procédures d'escalade, une atténuation rapide et des post-mortem documentés. Il informe rapidement les clients et publie les avis de sécurité sur https://streamlit.io/advisories .

    - Tests de pénétration
    Streamlit utilise des outils de sécurité tiers pour rechercher régulièrement les vulnérabilités. Nos partenaires de sécurité effectuent des tests de pénétration périodiques et intensifs sur la plate-forme Streamlit. Notre équipe de développement de produits répond immédiatement à tout problème identifié ou vulnérabilité potentielle pour garantir la qualité et la sécurité des applications Streamlit.

*NB : On notera également une limitation de la personnalisation de la mise en page de l'application.*

Concurrents
-----------

Les principales alternatives sont :

* **Dash** : Dash est un navigateur de documentation API et un gestionnaire d'extraits de code. Dash stocke des extraits de code et recherche instantanément des ensembles de documentation hors ligne pour plus de 150 API.
* **Jupyter** : Le Jupyter Notebook est une plateforme informatique interactive basée sur le Web. Le bloc-note combine du code en direct, des équations, du texte narratif, des visualisations, des tableaux interactifs et d'autres médias.
* **Flask** : Flask est conçu pour démarrer très rapidement et a été développé avec les meilleurs intentions à l'esprit.
* **Shiny** : Il s'agit d'un package R open source qui fournit un cadre web élégant et puissant pour créer des applications web. Il permet de transformer les analyses en application web interactives sans nécessiter de connaissance en HTML, CSS ou Javascript
* **Bokeh** : Bokeh est une bibliothèque de visualisation interactive pour les navigateurs Web modernes. Il fournit une construction élégante et concise de graphiques polyvalents et offre une interactivité haute performance sur des ensembles de données volumineux ou en continu.

