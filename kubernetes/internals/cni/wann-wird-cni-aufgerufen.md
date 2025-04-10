# Wann wird CNI aufgerufen.

 * CNI erstellt über einen Call zu CRI (CRI-Plugin in containerd) Sandbox
 * Für diese Sandbox wird der Network Namespace gesetzt und CNI aufgerufen, um eine PODIP zuzuordnen
 * Sandbox-Container (=Pause-Container) wird in dieser Sandbox und ist somit diesen Network Namespace zugerodnet
 * Alle weitere Container, die gestartet werden, gehören auch dieser Sandbox an.
 * Sandbox = Pod ist das gleiche

![image](https://github.com/user-attachments/assets/7823f1e7-ff6d-4d77-9c72-29de726e22e6)
