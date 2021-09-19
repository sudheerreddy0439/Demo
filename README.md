$expiresWithinDays = 365
$expired = Get-AzureADServicePrincipal -All $true | Where-Object {($_.Tags -contains "WindowsAzureActiveDirectoryGalleryApplicationNonPrimaryV1") -or ($_.Tags -contains "WindowsAzureActiveDirectoryCustomSingleSignOnApplication")} | ForEach-Object {
    $app = $_
        $id = "Not set"
        $KeyCredential = $_.KeyCredentials


        if($_.CustomKeyIdentifier) {
            $id = [System.Text.Encoding]::UTF8.GetString($_.CustomKeyIdentifier)
        }

        if ( $KeyCredential.EndDate -lt (Get-Date).AddDays($expiresWithinDays) ) {
        [PSCustomObject] @{
            App = $app.DisplayName
            ObjectID = $app.ObjectId
            AppId = $app.AppId
          #  Type = $_.GetType().name
           # KeyIdentifier = $id
            EndDate = $KeyCredential.EndDate
            }
        }
    }
 
$expired | Out-GridView



https://portal.azure.com/
