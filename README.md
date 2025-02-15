# Desafio-Benner

using System;
using System.Collections.Generic;

public class Network
{
    private List<int> Elements { get; set; }
    private List<(int, int)> Connections { get; set; }

    public Network(int elementQuantity)
    {
        if (elementQuantity <= 0)
            throw new Exception("O valor deve ser um inteiro positivo.");

        Elements = new List<int>();
        Connections = new List<(int, int)>();

        for (int i = 1; i <= elementQuantity; i++)
            Elements.Add(i);
    }

    public void Connect(int elementOne, int elementTwo)
    {
        if (elementOne <= 0 || elementTwo <= 0)
            throw new Exception("Os valores devem ser inteiros positivos.");

        if (!Elements.Contains(elementOne) || !Elements.Contains(elementTwo))
            throw new Exception("O valor não pertence ao conjunto.");

        var tuple = (elementOne, elementTwo);

        if (Connections.Contains(tuple))
            throw new Exception("Essa conexão já existe.");

        Connections.Add(tuple);
    }

    public void Disconnect(int elementOne, int elementTwo)
    {
        var tuple = (elementOne, elementTwo);

        if (!Connections.Contains(tuple))
            throw new Exception("Essa conexão não existe.");

        Connections.Remove(tuple);
    }

    public bool Query(int elementOne, int elementTwo)
    {
        var levelConnection = LevelConnection(elementOne, elementTwo);
            
        if(levelConnection == 1 || levelConnection >= 2) 
           return true;
        
        return false;
    }

    public int LevelConnection(int elementOne, int elementTwo)
    {
        if (Connections.Contains((elementOne, elementTwo)))
            return 1;

        foreach (var (index1, index2) in Connections)
        {
            if (elementOne == index1 || elementOne == index2)
            {
                int IntermediateConnection;
                if (elementOne == index1)
                    IntermediateConnection = index2;
                else
                    IntermediateConnection = index1;
        
                if (Connections.Contains((IntermediateConnection, elementTwo)) || Connections.Contains((elementTwo, IntermediateConnection)))
                    return 2;
            }
        }

        return 0;
    }
}
