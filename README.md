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

## 2. Podłoże teoretyczne / stack technologiczny
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

Architektura projektu składa się z czterech warstw:

**Warstwa aplikacyjna – AWS EKS.** Astronomy Shop uruchomiona jest w klastrze **Amazon EKS**. Każdy mikroserwis działa jako osobny Deployment w przestrzeni nazw `astronomy-shop` i jest instrumentowany przez **OpenTelemetry SDK**, generując metryki, logi oraz ślady (traces).

**Warstwa zbierania danych – Grafana Alloy.** Alloy działa jako agent zbierający dane telemetryczne z mikroserwisów przez protokół **OTLP**. Przetwarza dane i przesyła je do odpowiednich backendów w Grafana Cloud: metryki do Mimira, logi do Loki, ślady do Tempo.

**Warstwa przechowywania danych – Grafana Cloud.** Dane przechowywane są w zarządzanych usługach Grafana Cloud: **Mimir** (metryki), **Loki** (logi) i **Tempo** (ślady). Komunikacja z klastra odbywa się przez HTTPS z uwierzytelnianiem tokenem API.

**Warstwa wizualizacji – Grafana.** Grafana stanowi interfejs do analizy danych w formie dashboardów budowanych zapytaniami PromQL, LogQL i TraceQL. Tworzenie dashboardów wspierane jest przez **Grafana Assistant**, który na podstawie promptów użytkownika w języku naturalnym generuje gotowe panele.

```
┌─────────────────────────────────────────────────────────┐
│                      AWS EKS Cluster                    │
│  ┌────────────────────────────────────────────────────┐ │
│  │  Namespace: astronomy-shop                         │ │
│  │  frontend · cartservice · checkoutservice · ...    │ │
│  │           (OTLP SDK instrumentation)               │ │
│  └───────────────────┬────────────────────────────────┘ │
│                      │ OTLP                             │
│  ┌───────────────────▼────────────────────────────────┐ │
│  │  Namespace: monitoring – Grafana Alloy             │ │
│  └───────────┬──────────────────────────┬─────────────┘ │
└──────────────┼──────────────────────────┼───────────────┘
               │ HTTPS                    │ HTTPS
               ▼                          ▼
┌──────────────────────────┐  ┌──────────────────────────┐
│     Grafana Cloud        │  │     Grafana Cloud         │
│  Mimir  · Loki · Tempo   │  │  Grafana (UI + Assistant) │
└──────────────────────────┘  └──────────────────────────┘
```

## 6. Opis konfiguracji środowiska

### Wymagania wstępne

Do uruchomienia środowiska wymagane są następujące narzędzia zainstalowane lokalnie: `aws cli`, `eksctl`, `kubectl` oraz `helm`. Wymagane jest również konto **AWS** z uprawnieniami do tworzenia zasobów EKS, EC2, IAM i VPC, a także konto **Grafana Cloud**.

### Klaster AWS EKS

Środowisko demonstracyjne uruchamiane jest w regionie **us-east-1** z grupą węzłów opartych na instancjach **t3.medium** (3 węzły).

### Grafana Cloud

W panelu Grafana Cloud należy utworzyć nowy stos oraz wygenerować token API z uprawnieniami do zapisu metryk, logów i śladów. Adresy endpointów Mimira, Loki i Tempo są następnie używane w konfiguracji Alloy.

### Grafana Alloy

Alloy konfigurowany jest plikiem w formacie River, który definiuje pipeline zbierania danych: odbiór przez OTLP, przetwarzanie wsadowe i eksport do odpowiednich backendów Grafana Cloud. Dane uwierzytelniające przechowywane są jako Kubernetes Secret.

### Astronomy Shop

Aplikacja wdrażana jest przez oficjalny chart Helm z repozytorium OpenTelemetry. Wbudowany kolektor OpenTelemetry zostaje wyłączony, a endpoint OTLP mikroserwisów przekierowany na adres wewnętrznego serwisu Alloy w klastrze.

## 7. Metoda instalacji

Instalacja przebiega w czterech kolejnych krokach.

**Krok 1 – Przygotowanie klastra EKS.** Po skonfigurowaniu AWS CLI z danymi dostępowymi konta, klaster EKS tworzony jest poleceniem `eksctl create cluster` z przygotowanym plikiem konfiguracyjnym. Proces trwa ok. 15–20 minut, po czym `eksctl` automatycznie aktualizuje lokalny plik `kubeconfig`.

**Krok 2 – Wdrożenie Astronomy Shop.** Aplikacja instalowana jest przez Helm z oficjalnego repozytorium OpenTelemetry Demo. Podczas instalacji wbudowany kolektor jest wyłączany, a endpoint OTLP przekierowany na adres Alloy wewnątrz klastra.

**Krok 3 – Instalacja Grafana Alloy.** Alloy instalowany jest przez Helm z repozytorium Grafana. Przed instalacją tworzone są: Kubernetes Secret z danymi uwierzytelniającymi Grafana Cloud oraz ConfigMap z plikiem konfiguracyjnym Alloy.

**Krok 4 – Weryfikacja.** Po wdrożeniu poprawność integracji weryfikowana jest w Grafana Cloud przez sekcję **Explore** – sprawdzany jest napływ metryk (Mimir), logów (Loki) oraz śladów (Tempo) z przestrzeni nazw `astronomy-shop`.

## 8. Kroki wzdrożenia demo
### a. Przygotowanie konfiguracji
### b. Przygotowanie danych

## 9. Opis demo
### a. Procedura wykonania
### b. Prezentacja wyników

## 10. Podsumowanie i wnioski

## 11. Bibliografia
1. The Kubernetes Authors, *Production-Grade Container Scheduling and Management*, [https://kubernetes.io/](https://kubernetes.io/)
2. OpenTelemetry Authors, *OpenTelemetry Astronomy Shop, a microservice-based distributed system intended to illustrate the implementation of OpenTelemetry in a near real-world environment*, [https://opentelemetry.io/docs/demo/](https://opentelemetry.io/docs/demo/)
3. Grafana Labs, *Grafana Assistant*, [https://grafana.com/docs/grafana-cloud/machine-learning/assistant/](https://grafana.com/docs/grafana-cloud/machine-learning/assistant/)