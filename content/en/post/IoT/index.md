+++
author = "Hugo Authors"
title = "IoT capteur TTGO T-Display - M2"
date = "2023-09-30"
description = "Développement sur un capteur TTGO T-Display avec Arduino"
tags = [
    "Arduino",
    "Raspberry pi"
]
categories = [
    "IoT",
    "Arduino",
    "Master",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
image = "IoT.jpg"
toc = false
+++

## But

L'objectif du projet est d'implémenter une API RESTful sur un capteur TTGO T-Display.

L'API doit permettre de lister les capteurs (température/lumière) connectés à l'ESP32, de récupérer les informations associées à ce ou ces capteurs,  de contrôler l'allumage, l'extinction, le changement d'état d'une LED connectée à l'ESP32, de définir un seuil permettant d'allumer/éteindre la LED en fonction de la valeur captée par un des capteurs connecté à l'ESP32.

Le respect des règles classiques (usuelles) de développement logiciel sont également attendues (programmation modulaire, commentaires, tests unitaires, documentation, ...).

Ce projet est en cours pour ma dernière année de Master.