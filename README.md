# wazuh-active-response-lab
Automated Threat Detection and Incident Response using Wazuh SIEM

Bu layihədə mərkəzləşdirilmiş loq monitorinqi, real zamanlı kibertəhlükəsizlik analizi və avtomatik təhdidlərin qarşısının alınması üçün Wazuh SIEM mühiti qurulmuşdur. Layihə çərçivəsində Linux agentlərinə qarşı edilən aqressiv SSH Brute-Force hücumları (Hydra ilə) simulyasiya edilmiş, Wazuh Server tərəfindən Rule Level 10 təhlükə səviyyəsi ilə uğurla analiz olunmuş və Active Response (Firewall-Drop) mexanizmi işə salınaraq hücumçu real zamanlı olaraq şəbəkə səviyyəsində bloklanmışdır.


İstifadə Olunan Texnologiyalar və Alətlər

SIEM / SOC: Wazuh Manager & Wazuh Dashboard
Hədəf / Agentlər (Victim): Kali Linux (VirtualBox)
Hücum Simulyasiyası (Attacker): Kali Linux (VMware), THC-Hydra
Müdafiə / Şəbəkə: Linux Iptables Firewall, Bash, XML Konfiqurasiyası


Şəbəkə Arxitekturası və İş Məntiqi (Architecture Workflow)

Hücum və müdafiə ssenarisinin işləmə alqoritmi aşağıdakı mərhələlərdən ibarətdir:
Hücum Mərhələsi: 
Hücumçu Kali Linux-dan Hydra aləti vasitəsilə qurban Kali-nin SSH portuna (port 22) qarşı brute-force hücumu başladır.
Loq Toplanması: 
Qurban maşındakı sshd-session xidməti uğursuz daxilolma cəhdlərini auth.log faylına yazır. Wazuh Agent bu loqları real zamanlı olaraq oxuyur və şifrəli xətt üzərindən Wazuh Serverə göndərir.
Analiz və Korrelyasiya: 
Wazuh Server daxil olan loqları süzgəcdən keçirir. Ard-arda gələn uğursuz cəhdləri analiz edərək təhlükə səviyyəsini Level 10 (Custom Rule / Ruleset uyuşmazlığı) olaraq təyin edir və dərhal alert (həyəcan siqnalı) yaradır.
Aktiv Müdafiə (Active Response): 
Server əvvəlcədən təyin olunmuş ossec.conf qaydasına əsasən qurban agentə əmr göndərir. Qurban maşındakı agent firewall-drop skriptini işə salaraq hücumçu IP-ni Linux-un daxili iptables firewall siyahısına DROP qaydası ilə əlavə edir. Hücumçunun şəbəkə bağlantısı (Ping və SSH daxil olmaqla) 10 dəqiqəlik kəsilir.


Konfiqurasiya və Sənədləşdirmə (Implementation)

<img width="550" height="128" alt="Screenshot 2026-06-06 221955" src="https://github.com/user-attachments/assets/97a5b9ca-ee8e-4ace-b34f-08cf11245484" />


Əldə Olunan Nəticələr və Sübutlar (Key Results & Artifacts)


Sübut 1 (Hücum): Hücumçu Hydra ilə SSH Brute-Force hücumu edir.

<img width="632" height="220" alt="Screenshot 2026-06-06 223608" src="https://github.com/user-attachments/assets/f26b526e-ac9f-4a4d-bdd3-03424186accb" />




Sübut 2 (Yerli Müdafiə): Agent maşında active-responses.log və iptables -L -n -v əmrlərinin çıxışında hücumçu IP bloklanır və ping getmir.

<img width="1917" height="519" alt="Screenshot 2026-06-06 223744" src="https://github.com/user-attachments/assets/453f9dd4-f5e1-4124-b2e3-8bf7a126fc31" />


<img width="742" height="210" alt="Screenshot 2026-06-06 223809" src="https://github.com/user-attachments/assets/be49872d-f7df-4c70-9841-7e87baf8e84c" />


<img width="589" height="135" alt="Screenshot 2026-06-06 224253" src="https://github.com/user-attachments/assets/afe9e085-33bd-4c9b-8132-d31d9caf9037" />



Sübut 3 (SOC Dashboard Analizi): Wazuh Dashboard-da data.command:

<img width="1919" height="1015" alt="Screenshot 2026-06-06 231247" src="https://github.com/user-attachments/assets/cf3356a0-8495-4158-9d5e-6a9bdffd8bc2" />

İnsidentin SIEM panelində Rule 5551 (Level 10) və MITRE ATT&CK T1110 (Brute Force) etiketi ilə uğurla qeydə alınır.
