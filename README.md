# Dash-M - Dashboard creation and management with Grafana Assistance

**Autorzy:**
  * Artur Gęsiarz
  * Michał Czajor
  * Robert Mesek
  * Robert Zuziak

**2026, grupa 3**

---

## Spis treści
1. [Wstęp](#1-wstęp) <!-- Introduction -->
2. [Podłoże teoretyczne / stack technologiczny](#2-podłoże-teoretyczne--stack-technologiczny) <!-- Theoretical background/technology stack -->
3. [Opis koncepcji case study](#3-opis-koncepcji-case-study) <!-- Case study concept description (Application/Observability/Vizualization) -->
4. [Wysokopoziomowa architektura](#4-wysokopoziomowa-architektura) <!-- Case study high level architecture -->
5. [Szczegółowa architektura](#5-szczegółowa-architektura) <!-- Case study detailed architecture -->
6. [Opis konfiguracji środowiska](#6-opis-konfiguracji-środowiska) <!-- Environment configuration description -->
7. [Metoda instalacji](#7-metoda-instalacji) <!-- Installation method -->
8. [Kroki wzdrożenia demo](#8-kroki-wzdrożenia-demo) <!-- Demo deployment steps: a. Configuration set-up b. Data preparation -->
9. [Opis demo](#9-opis-demo) <!-- Demo description a. Execution procedure b. Results presentation – All prompts used with AI models should be listed, screens from Grafana dashboard should be attached. -->
10. [Podsumowanie i wnioski](#10-podsumowanie-i-wnioski) <!-- Summary – conclusions -->
11. [Bibliografia](#11-bibliografia) <!-- References -->

---

## 1. Wstęp
Celem projektu jest zaprezentowanie wykorzystania narzędzi observability oraz wsparcia modeli AI w procesie monitorowania aplikacji działającej w środowisku chmurowym. W projekcie zostanie wykorzystana przykładowa aplikacja **Astronomy Shop**, która będzie generować dane telemetryczne wizualizowane następnie w systemie monitoringu.

## 2. Podłoże teoretyczne / Stack technologiczny
Projekt opiera się na technologiach związanych z konteneryzacją, orkiestracją oraz obserwowalnością systemów. Aplikacja zostanie uruchomiona w środowisku **Docker** oraz **Kubernetes** na platformie **AWS**. Dane telemetryczne będą zbierane przy użyciu **OpenTelemetry** oraz monitorowane przez systemy takie jak **Prometheus**, a wizualizacja i analiza danych zostanie zrealizowana w **Grafanie** z wykorzystaniem **Grafana Assistant**.

## 3. Opis koncepcji case study

### Aplikacja
W projekcie wykorzystana zostanie przykładowa aplikacja demonstracyjna **Astronomy Shop**, uruchomiona w klastrze Kubernetes. Aplikacja będzie generować ruch oraz dane telemetryczne umożliwiające analizę działania systemu.

### Observability
Do zbierania metryk, logów oraz śladów wykorzystywane będzie **OpenTelemetry**. Dane te będą następnie przetwarzane i przechowywane w systemie monitorowania umożliwiającym ich dalszą analizę.

### Wizualizacja
Zebrane dane telemetryczne będą wizualizowane w **Grafanie** w postaci dashboardów monitorujących działanie aplikacji. Do tworzenia oraz modyfikacji dashboardów wykorzystany zostanie **Grafana Assistant**, który wspiera użytkownika przy pomocy modeli językowych.

## 4. Wysokopoziomowa architektura
Architektura rozwiązania składa się z kilku głównych warstw. Aplikacja uruchomiona w klastrze Kubernetes generuje dane telemetryczne zbierane przez OpenTelemetry. Dane te trafiają do warstwy observability, gdzie są przetwarzane i przechowywane. Następnie są one wizualizowane w Grafanie w formie dashboardów. Proces tworzenia i zarządzania dashboardami wspierany jest przez Grafana Assistant wykorzystujący model LLM.

<img src="docs/high-level-architecture.svg" alt="Wysokopoziomowa architektura" width="500"/>

## 5. Szczegółowa architektura

## 6. Opis konfiguracji środowiska

## 7. Metoda instalacji

## 8. Kroki wzdrożenia demo
### a. Przygotowanie konfiguracji
### b. Przygotowanie danych

## 9. Opis demo
### a. Procedura wykonania
### b. Prezentacja wyników

## 10. Podsumowanie i wnioski

## 11. Bibliografia
