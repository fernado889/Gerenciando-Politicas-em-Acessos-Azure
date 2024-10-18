# Gerenciando-Politicas-em-Acessos-Azure
az login

{
    "mode": "Indexed",
    "policyRule": {
        "if": {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
        },
        "then": {
            "effect": "deny",
            "details": {
                "field": "Microsoft.Compute/virtualMachines/sku.name",
                "notIn": [
                    "Standard_B1s",
                    "Standard_B2s"
                ]
            }
        }
    },
    "parameters": {}
}

az policy definition create --name denyVMsPolicy --rules denyVMPolicy.json --display-name "Deny Specific VM SKUs"

az policy assignment create --policy denyVMsPolicy --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
Connect-AzAccount
$policyRule = @"
{
    "if": {
        "field": "type",
        "equals": "Microsoft.Compute/virtualMachines"
    },
    "then": {
        "effect": "deny",
        "details": {
            "field": "Microsoft.Compute/virtualMachines/sku.name",
            "notIn": [
                "Standard_B1s",
                "Standard_B2s"
            ]
        }
    }
}
"@

$policyDefinition = New-AzPolicyDefinition -Name "denyVMsPolicy" -Policy $policyRule -DisplayName "Deny Specific VM SKUs"

New-AzPolicyAssignment -Name "denyVMsAssignment" -PolicyDefinition $policyDefinition -Scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
