###########################################################
#   Aventus - All-round Medewerker Support Systems and    #
#   Devices - Omgevingsinstaller 2022                     #
#   Installeert: Gmetrix SMS7, VMWare Workstation 16      #
#   Microsoft Teams.                                      #
###########################################################

###########################################################
#           Dion "Glazey" Sorel - 25605OLVM               # 
#                      2022 - 2023                        #
#                Credit where it is due:                  #
#          StackOverflow: Een hoop voorbeelden            #
#          Microsoft: Hun geweldige documentatie          #
#       LazyAdmin: Voorbeelden voor het downloaden        #
#    VMWare: Documentatie met betrekking tot installer    #
###########################################################

# Wat omgevingsvariabelen
$Programmas = @{Gmetrix = "https://releases.gmetrix.net/smse/latest/win/GMetrixSMSe-v7.0.25.exe"; Vmware = "https://www.vmware.com/go/getworkstation-win"; Teams = "https://go.microsoft.com/fwlink/p/?LinkID=2187327&clcid=0x413&culture=nl-nl&country=NL"}
$ProgrammaIndex = "Gmetrix", "Vmware", "Teams"
$Gedownload = @{Gmetrix = 'False'; Vmware = 'False'; Teams = 'False'}
$ExeNamen = @{Gmetrix = "gmetrixsms7.exe"; Vmware = "vmwarew16pro.exe"; Teams = "MicrosoftTeams.exe"}
$ExeHashes = @{Gmetrix = 'BDAA16285C94B429BF98F1A17C15C8602C48E677E74C3622C0222647C6B7CB66'; Vmware = '758F7211D631B2B5B52DF7214485FE2082661E5BA18054C8D91BE0D7E27DBB2F'; Teams = 'D6F5D7D5C3E1CC6501D3363A765B5FE96F3467AC5CA6B2826C5A41D6851236BA'}
$SetupOpties = "/v /qn EULAS_AGREED=1 SERIALNUMBER='XXXXX-XXXXX-XXXXX-XXXXX-XXXXX'-* AUTOSOFTWAREUPDATE=1"

# Welkom Text
Write-Host("Aventus Programma Installatie Script")
Write-Host("Versie 0.1-'cougar'")

# Check of de bestanden er al zijn
foreach ($element in $ProgrammaIndex) {
    Test-Path -Path $ExeNamen[$element]
    if((Test-Path -Path $ExeNamen[$element]) -eq 'True') {
        Write-Host("$element is al gedownload.")
        $Gedownload[$element] = 'True'
    }
}

# Check of de gedownloade bestanden correct zijn:
foreach ($element in $ProgrammaIndex){
    if ($Gedownload[$element] -eq 'True') {
        if((Get-FileHash -Path $ExeNamen[$element] | Select-Object -ExpandProperty Hash) -eq $ExeHashes[$element]){
            Write-Host("$element is correct gedownload!")
    
        }else{
            Write-Host("$element is niet correct gedownload!")
            $Gedownload[$element] = 'False'
        }
    }
}


# Download gedeelte
foreach ($element in $ProgrammaIndex) {
    try {
        if ($Gedownload[$element] -eq 'False') {
            Write-Host("Poging om $element te downloaden.")
            # We gebruiken Invoke-WebRequest om te downloaden.
            Invoke-WebRequest -Uri $Programmas[$element] -OutFile $ExeNamen[$element]
            # Zet de flag voor gedownload naar true.
            $Gedownload[$element] = "True"
        } else {
            Write-Host("Programma al gedownload, we gaan verder naar het installatie proces!")
        }
        
    } catch {
        Write-Host("$element kon niet gedownload worden door: $_")
        Throw "Download gefaald voor: $element"
    }
}

# Installeer gedeelte
foreach ($element in $ExeNamen.Values){
    try {
        if($element -eq 'vmwarew16pro.exe') {
            Write-Host($element + " wordt geinstalleerd met de opties: " + $SetupOpties)
            Start-Process $element -ArgumentList $SetupOpties -Wait
        } else {
            Write-Host($element + " wordt geinstalleerd")
            Start-Process $element -Wait
        }
    } catch {
        Write-Host("$element Kon niet worden geinstalleerd, en gaf de volgende rede: $_")
        Throw "Installatie geannuleerd voor: $element"
    }    
}
