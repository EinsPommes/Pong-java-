/**
 * Die Klasse Ball stellt den Ball am Bildschirm dar, bewegt ihn, reagiert auf Kollision
 * mit den Schlägern und dem oberen/unteren Bildschirmrand. Sie registriert auch, ob der Ball
 * den sichtbaren Bereich rechts oder links verlässt und veranlasst dann die Änderung des Spielstands 
 * sowie einen Abstoß in der Mitte des Bildschirms.
 */
class Ball extends Rectangle {
   
   private double vx;   // x-Komponente der Ballgeschwindigkeit (in Pixel je 1/30 s)
   private double vy;   // y-Komponente der Ballgeschwindigkeit

   private double breite = 20;   // Breite und zugleich Höhe des Ball-Rechtecks

   private Schläger schlägerLinks;  // Referenz auf das linke Schläger-Objekt
   private Schläger schlägerRechts; // Referenz auf das rechte Schläger-Objekt

   private Pong pong;               // Referenz auf das Pong-Objekt

   /**
    * Der Konstruktor der Klasse Ball initialisiert das Ball-Rechteck.
    */
   public Ball(Schläger schlägerLinks, Schläger schlägerRechts, Pong pong) {
      super(400 - breite / 2, 300 - breite / 2, breite, breite);  // Aufruf des Konstruktors der Oberklasse Rectangle
      setzeZufallsGeschwindigkeit();
      setFillColor(Color.white);

      this.schlägerRechts = schlägerRechts;
      this.schlägerLinks = schlägerLinks;
      this.pong = pong;
   }

   /**
    * Die Methode act wird vom Browser 30-mal pro Sekunde aufgerufen. Sie bewegt den Ball und
    * reagiert auf Kollisionen mit den Schlägern, dem oberen/unteren Rand sowie auf das Verlassen
    * des Grafikbereichs rechts/links.
    */
   public void act() {
      move(vx, vy);  // bewegt den Ball
      
      // stößt der Ball am oberen/unteren Rand an? => vy umkehren
      if(getCenterY() < breite / 2 || getCenterY() > getWorld().getHeight() - breite / 2) {
         vy *= -1;
      } 

      testKollisionMitSchläger(schlägerLinks);
      testKollisionMitSchläger(schlägerRechts);

      // Wenn sich der Ball schon um vx weiter links befindet als bei Berührung mit dem linken Schläger,
      // dann lassen wir kein Abprallen mit dem Schläger mehr zu, sondern werten es als Punkt für den 
      // rechten Spieler:
      if(getCenterX() < schlägerLinks.getWidth() + breite / 2 - Math.abs(vx)) {
         moveTo(400, 300);             // Abstoß in der Mitte des Grafikbereichs
         setzeZufallsGeschwindigkeit();
         pong.punktFürRechtenSpieler();
      }
      
      // ... entsprechend für den rechten Bildschirmrand:
      if(getCenterX() > getWorld().getWidth() - schlägerRechts.getWidth() - breite / 2 + Math.abs(vx)) {
         moveTo(400, 300);
         setzeZufallsGeschwindigkeit();
         pong.punktFürLinkenSpielerNeu();
      }

   }

   private void testKollisionMitSchläger(Schläger schläger) {
      if(this.collidesWith(schläger)) {
         vx *= -1;
         double dy = schläger.getCenterY() - getCenterY();
         if(Math.abs(vy) < 10) {
            vy -= dy / schläger.getHeight() * 6; 
         }
         if(schläger == schlägerLinks) {
            Sound.playSound(Sound.pong_f);
         } else {
            Sound.playSound(Sound.pong_d);
         }

      }
   }

   /**
    * Diese Methode gibt dem Ball eine zufällige Geschwindigkeit. Dabei dürfen aber keine Geschwindigkeitsvektoren
    * entstehen, die zu "flach" oder "steil" sind, da das Spiel sonst langweilig wird.
    */
   public void setzeZufallsGeschwindigkeit() {
      double v = Math.random() * 4 + 8;      // Betrag der Geschwindigkeit zwischen 8 und 12 (Pixel je 1/30 s)
      double winkel = Math.random() * Math.PI / 4 + Math.PI / 8;  // Winkel zwischen 22,5 und 67,5 Grad
      vx = Math.cos(winkel) * v;
      vy = Math.sin(winkel) * v;
      if(Math.random() < 0.5) vx *= -1;   // mit 50% Wahrscheinlichkeit: Spiegeln an der y-Achse
      if(Math.random() < 0.5) vy *= -1;   // mit 50% Wahrscheinlichkeit: Spiegeln an der x-Achse
   }

}
