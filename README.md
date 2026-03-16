# Projekt: Observability aplikacji z wykorzystaniem OpenTelemetry i Grafana Assistant Dash-M


# 1. Wstęp
Celem projektu jest zaprezentowanie wykorzystania narzędzi observability oraz wsparcia modeli AI w procesie monitorowania aplikacji działającej w środowisku chmurowym. W projekcie zostanie wykorzystana przykładowa aplikacja **Astronomy Shop**, która będzie generować dane telemetryczne wizualizowane następnie w systemie monitoringu.

# 2. Podłoże teoretyczne / stack technologiczny
Projekt opiera się na technologiach związanych z konteneryzacją, orkiestracją oraz obserwowalnością systemów. Aplikacja zostanie uruchomiona w środowisku **Docker** oraz **Kubernetes** na platformie **AWS**. Dane telemetryczne będą zbierane przy użyciu **OpenTelemetry** oraz monitorowane przez systemy takie jak **Prometheus**, a wizualizacja i analiza danych zostanie zrealizowana w **Grafanie** z wykorzystaniem **Grafana Assistant**.

# 3. Case study 

## Aplikacja
W projekcie wykorzystana zostanie przykładowa aplikacja demonstracyjna **Astronomy Shop**, uruchomiona w klastrze Kubernetes. Aplikacja będzie generować ruch oraz dane telemetryczne umożliwiające analizę działania systemu.

## Observability
Do zbierania metryk, logów oraz śladów wykorzystywane będzie **OpenTelemetry**. Dane te będą następnie przetwarzane i przechowywane w systemie monitorowania umożliwiającym ich dalszą analizę.

## Wizualizacja
Zebrane dane telemetryczne będą wizualizowane w **Grafanie** w postaci dashboardów monitorujących działanie aplikacji. Do tworzenia oraz modyfikacji dashboardów wykorzystany zostanie **Grafana Assistant**, który wspiera użytkownika przy pomocy modeli językowych.

# 4. Wysoko poziomowa architektura
Architektura rozwiązania składa się z kilku głównych warstw. Aplikacja uruchomiona w klastrze Kubernetes generuje dane telemetryczne zbierane przez OpenTelemetry. Dane te trafiają do warstwy observability, gdzie są przetwarzane i przechowywane. Następnie są one wizualizowane w Grafanie w formie dashboardów. Proces tworzenia i zarządzania dashboardami wspierany jest przez Grafana Assistant wykorzystujący model LLM.