public static void Method(List<Account> accList){
        Set<Id> accIds = new Set<Id>();
        for(Account acc : accList){
            if(acc.Closed_All_Opportunities__c == true){
                accIds.add(acc.Id);
            }
        }
        List<Opportunity> oppList = new List<Opportunity>();
        for(Account acc : [SELECT Id , Closed_All_Opportunities__c , (SELECT Id , Probability FROM Opportunities) FROM Account WHERE Id IN : accIds]){
            for(Opportunity Opp : acc.Opportunities){
                if(Opp.Probability >= 70){
                    Opp.StageName = 'Closed Won	';
                    Opp.CloseDate = System.TODAY();
                    oppList.add(Opp);
                }
            }
        }
        if(oppList.size()>0){
            update oppList;
        }
    }
