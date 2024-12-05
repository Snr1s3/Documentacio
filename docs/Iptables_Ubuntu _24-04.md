Instal·lació de Iptables

## Instal·lar Iptables
```bash
sudo apt install iptables iptables-persistent
```
#### Exemple
```bash
sudo iptables [option] CHAIN_rule [-j target]
```
-A —append: Afegiex una regla a una cadena.   
-C —check : Cerca una regla que coincideixi amb els requeriments de la cadena.      
-D —delete: Esborra les regles especificades d'una cadena.      
-F —flush: Elimina totes les regles.      
-I —insert: Afegeix una regla a una cadena en una posició donada.             
-L —list : Mostra totes les regles d'una cadena.          
-N -new-chain: Crea una nova cadena.           
-v —verbose: Mostra més informació quan s'usa una opció de llista.            
-X —delete-chain: Elimina la cadena proporcionada.            
## Comprovar estat actual
```bash
sudo iptables -L
```
## Controlar el transit per direcció IP
```bash
sudo iptables -A INPUT -s direcció_IP_a_autorizar -j ACCEPT
sudo iptables -A INPUT -s dirección_IP_a_bloquejar -j DROP
sudo iptables -A INPUT -m iprange --src-range direcció_IP_inici-direcció_IP_final -j REJECT
```
## Autoritzar transit al localhost
```bash
sudo iptables -A INPUT -i lo -j ACCEPT
```

## Autoritzar el transit a ports específics
```bash
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

## Eliminar el transit no desitjat
```bash
sudo iptables -D INPUT <Number>
```

## Eliminar una regla
```bash
sudo iptables -P INPUT DROP
sudo iptables -L --line-numbers
sudo iptables -D INPUT <Number>
```

## Guardar els canvis
```bash
sudo iptables-save > /etc/iptables/rules.v4
```
