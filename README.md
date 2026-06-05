# Home Assistant repo
Home Assistant blueprints, scripts en dergelijke

## Automation blueprint - Peblar slim opladen
Met behulp van deze blueprint kan een Peblar laadpaal slim aangestuurd worden op basis van seizoen en dynamische tarieven.
Hierbij heb je ook nodig:
- De officiële Peblar integratie: https://www.home-assistant.io/integrations/peblar/
- De officiële Nord Pool integratie: https://www.home-assistant.io/integrations/nordpool/
- De Cheapest Energy Hours blueprints van TheFes: https://github.com/TheFes/cheapest-energy-hours

Voordat je de blueprint kunt gebruiken, dien je:
1. Een template sensor 'sensor.nordpool_ceh_prices' aan te maken in configuration.yaml die zorgt dat de prijzen als attribuut uit te lezen zijn: https://github.com/TheFes/cheapest-energy-hours/blob/main/documentation/blueprints/energy_price_sensor.md,
2. Een template helper van type binaire sensor aan te maken die true/false geeft op basis van dynamische tarieven, bijvoorbeeld met de volgende code:
```
{% from 'cheapest_energy_hours.jinja' import cheapest_energy_hours %}
{{ cheapest_energy_hours(sensor='sensor.nordpool_ceh_prices',hours=states('input_number.aantal_uren_goedkoop_laden'),start=states('input_datetime.starttijd_goedkoop_laden'),end=states('input_datetime.eindtijd_goedkoop_laden'),mode='is_now') }}
```
3. In het voorbeeld bij stap 2 is de binaire sensor dynamisch aanpasbaar qua sessieduur, starttijd en eindtijd, hiervoor zijn de volgende helpers nodig:
   - input_number.aantal_uren_goedkoop_laden: Numerieke invoer met minimale waarde 0.25, maximum waarde (bijvoorbeeld 5) en stapgrootte 0.25,
   - input_datetime.starttijd_goedkoop_laden: Datum & Tijd invoer helper,
   - input_datetime.eindtijd_goedkoop_laden: Datum & Tijd invoer helper,
4. Tot slot zijn de volgende helpers nodig:
   -  Schakelaar - Na zonsondergang laden (om te bepalen of er na zonsondergang geladen mag worden),
   -  Schakelaar - Zonneladen goedkoop aanvullen (om te bepalen of het zonneladen goedkoop aangevuld moet worden (schakelt op goedkoop tarief naar "Snel zonneladen")),
   -  Schakelaar - Direct laden (om direct te starten met laden wanneer nodig),
   -  Keuzelijst - Laadpaal gestart door - helper die waardes "Goedkoop laden, Zonneladen, Direct" moet bevatten om te onthouden welke optie de laadsessie gestart heeft.

# Bedankt:
* [@TheFes](https://github.com/TheFes) voor ontwikkelen van de Cheapest Energy Hours code!
