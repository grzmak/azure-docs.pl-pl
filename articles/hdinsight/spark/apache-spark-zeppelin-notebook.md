---
title: Korzystanie z notesów Zeppelin w klastrze platformy Apache Spark w usłudze Azure HDInsight
description: Instrukcje krok po kroku na temat korzystania z notesów Zeppelin z klastrami Apache Spark w usłudze Azure HDInsight.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.openlocfilehash: 8e32939f3e253bfdd6f8d989f616f30e1b9f27eb
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364867"
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Korzystanie z notesów Zeppelin w klastrze platformy Apache Spark w usłudze Azure HDInsight

Klastry HDInsight Spark obejmują z notesów Zeppelin używane do uruchamiania zadań platformy Spark. W tym artykule dowiesz się, jak używać Notes Zeppelin w klastrze usługi HDInsight.

**Wymagania wstępne:**

* Subskrypcja platformy Azure. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Klaster Apache Spark w usłudze HDInsight. Aby uzyskać instrukcje, zobacz [Tworzenie klastra platformy Apache Spark w usłudze Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Uruchom Notes Zeppelin
1. W bloku klastra Spark kliknij **pulpit nawigacyjny klastra**, a następnie kliknij przycisk **Notes Zeppelin**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klastra.
   
   > [!NOTE]
   > Można również przejść Notes Zeppelin dla klastra, otwierając następujący adres URL w przeglądarce. Zastąp ciąg **CLUSTERNAME** nazwą klastra:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
1. Utwórz nowy notes. Okienko nagłówka kliknij **notesu**, a następnie kliknij przycisk **Tworzenie nowej notatki**.
   
    ![Utwórz nowy notes Zeppelin](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "Utwórz nowy notes Zeppelin")
   
    Wprowadź nazwę notesu, a następnie kliknij przycisk **tworzenie Uwaga**.
1. Ponadto upewnij się, że nagłówek notesu wyświetlany jest stan połączonych. Jest on wskazywane przez zieloną kropkę, w prawym górnym rogu.
   
    ![Stan Notes Zeppelin](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "stan Notes Zeppelin")
1. Załaduj przykładowe dane do tabeli tymczasowej. Podczas tworzenia klastra Spark w HDInsight, przykładowy plik danych **hvac.csv**, jest kopiowany do skojarzonego konta magazynu w ramach **\HdiSamples\SensorSampleData\hvac**.
   
    Wklej poniższy fragment kodu w pusty akapit, który jest tworzony domyślnie w nowy notes.
   
        %livy2.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter
   
        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    Naciśnij klawisz **SHIFT + ENTER** lub kliknij przycisk **Odtwórz** przycisk akapitu uruchomić fragment kodu. Stanu w prawym rogu akapitu powinien postępu z gotowych do czasu działania na ZAKOŃCZONE. Dane wyjściowe pojawia się w dolnej części tego samego akapitu. Zrzut ekranu wygląda podobnie do poniższego:
   
    ![Tworzenie tabeli tymczasowej na podstawie danych pierwotnych](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "utworzyć tabelę tymczasową na podstawie nieprzetworzonych danych")
   
    Można również dołączyć tytuł do każdego akapitu. W prawym rogu kliknij **ustawienia** ikonę, a następnie kliknij przycisk **Pokaż tytuł**.

> [!NOTE]
> % spark2 interpreter nie jest obsługiwana w z notesów Zeppelin we wszystkich wersjach HDInsight i % sh interpreter nie będą obsługiwane z HDInsight 4.0 lub nowszy.
>

1. Możesz teraz uruchomić instrukcji rozwiązania Spark SQL **hvac** tabeli. Wklej poniższe zapytanie nowy akapit. Zapytanie pobiera identyfikator budynku i różnica między elementem docelowym i rzeczywiste temperatur w danym dniu każdego tworzenia. Naciśnij klawisz **SHIFT + ENTER**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    **% Sql** instrukcji na początku informuje Notes, aby użyć interpreter Scala usługi Livy.
   
    Poniższy zrzut ekranu przedstawia dane wyjściowe.
   
    ![Uruchamianie instrukcji rozwiązania Spark SQL za pomocą notesu](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "uruchamianie instrukcji rozwiązania Spark SQL za pomocą notesu")
   
     Kliknij opcje wyświetlania (wyróżnione w prostokącie), aby przełączać się między różne reprezentacje tych samych danych wyjściowych. Kliknij przycisk **ustawienia** wybrać jakie consitutes klucza i wartości w danych wyjściowych. Przechwytywanie ekranu powyżej używa **buildingID** jako klucz i średnią **temp_diff** jako wartość.
1. Można również uruchomić instrukcji rozwiązania Spark SQL za pomocą zmiennych w zapytaniu. Następny fragment kodu przedstawia sposób definiowania zmiennej, która **Temp**, w zapytaniu możliwych wartości chcesz wykonywać zapytania za pomocą. Przy pierwszym uruchomieniu zapytania, listy rozwijanej zostanie automatycznie wypełniona wartości, które określona dla zmiennej.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    Wklej ten fragment kodu w nowy akapit, a następnie naciśnij klawisz **SHIFT + ENTER**. Poniższy zrzut ekranu przedstawia dane wyjściowe.
   
    ![Uruchamianie instrukcji rozwiązania Spark SQL za pomocą notesu](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "uruchamianie instrukcji rozwiązania Spark SQL za pomocą notesu")
   
    Dla kolejnych zapytań możesz wybrać nową wartość z listy rozwijanej i ponownie uruchom zapytanie. Kliknij przycisk **ustawienia** wybrać jakie consitutes klucza i wartości w danych wyjściowych. Używa zrzucie ekranu powyżej **buildingID** jako klucz średnią **temp_diff** z wartością i **targettemp** jako grupa.
1. Uruchom ponownie interpreter Livy, aby zakończyć działanie aplikacji. Aby to zrobić, Otwórz okno Ustawienia interpreter, klikając zalogowanego w polu Nazwa użytkownika w prawym górnym rogu, a następnie kliknij przycisk **Interpreter**.
   
    ![Uruchom interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive danych wyjściowych")
1. Przewiń do usługi Livy interpreter ustawienia, a następnie kliknij przycisk **ponowne uruchomienie**.
   
    ![Uruchom ponownie Livy intepreter](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "ponowne uruchomienie Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Jak korzystanie z zewnętrznych pakietów z notesem?
Notes Zeppelin w klastrze Apache Spark na HDInsight (Linux) można skonfigurować na korzystanie z pakietów zewnętrznych, przez społeczność, które nie są uwzględniane out-of--box w klastrze. Możesz wyszukiwać [repozytorium Maven](http://search.maven.org/) uzyskać pełną listę pakietów, które są dostępne. Możesz również uzyskać listę dostępnych pakietów z innych źródeł. Na przykład pełną listę pakietów przez społeczność jest dostępne pod adresem [pakietów platformy Spark](http://spark-packages.org/).

W tym artykule pokazano, jak używać [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) pakietu z notesem Jupyter.

1. Otwórz ustawienia interpretera. W prawym górnym rogu, kliknij nazwę użytkownika zalogowanego, a następnie kliknij przycisk **Interpreter**.
   
    ![Uruchom interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive danych wyjściowych")
1. Przewiń do usługi Livy interpreter ustawienia, a następnie kliknij przycisk **Edytuj**.
   
    ![Zmień ustawienia interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "zmiany ustawień interpretera")
1. Dodaj nowy klucz o nazwie **livy.spark.jars.packages** i ustawić jej wartość w formacie `group:id:version`. Tak, jeśli chcesz używać [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) pakietu, należy ustawić wartość tego klucza do `com.databricks:spark-csv_2.10:1.4.0`.
   
    ![Zmień ustawienia interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "zmiany ustawień interpretera")
   
    Kliknij przycisk **Zapisz** a następnie ponownie uruchomić interpretera usługi Livy.
1. **Porada**: Jeśli chcesz dowiedzieć się, jak na wartość klucza podanymi powyżej, poniżej przedstawiono sposób.
   
    a. Zlokalizuj pakiet w repozytorium narzędzia Maven. Na potrzeby tego samouczka użyliśmy [spark csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Z repozytorium, Zbierz wartości **GroupId**, **ArtifactId**, i **wersji**.
   
    ![Korzystanie z zewnętrznych pakietów z notesem Jupyter](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "korzystanie z zewnętrznych pakietów z notesu programu Jupyter")
   
    c. Łączenie z trzech wartości, rozdzielone średnikiem (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Gdzie są zapisywane z notesów Zeppelin
Notesów Zeppelin są zapisywane w głównymi węzłami klastra. Aby usunąć klaster, jak również zostaną usunięte notesów. Jeśli chcesz zachować notesy do późniejszego użycia w innych klastrach, należy wyeksportować je po zakończeniu działania zadania. Aby wyeksportować Notes, kliknij **wyeksportować** ikony, jak pokazano na poniższej ilustracji.

![Pobieranie notesu](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "pobieranie notesu")

Spowoduje to zapisanie Notes jako plik JSON w lokalizacji pobierania.

## <a name="livy-session-management"></a>Zarządzanie sesjami programu Livy
Po uruchomieniu pierwszego akapitu kodu w notesie Zeppelin w klastrze usługi HDInsight Spark jest utworzona nowa sesja usługi Livy. Ta sesja jest współużytkowany przez wszystkich notesów Zeppelin, które utworzysz. Jeśli z jakiegoś powodu Livy sesja jest zabite (ponowne uruchomienie klastra itp.), nie można uruchomić zadania z Notes Zeppelin.

W takim przypadku należy wykonać następujące kroki, przed rozpoczęciem, uruchamia wszystkie zadania z Notes Zeppelin. 

1. Uruchom ponownie usługi Livy interpreter z Notes Zeppelin. Aby to zrobić, Otwórz okno Ustawienia interpreter, klikając zalogowanego w polu Nazwa użytkownika w prawym górnym rogu, a następnie kliknij przycisk **Interpreter**.
   
    ![Uruchom interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive danych wyjściowych")
1. Przewiń do usługi Livy interpreter ustawienia, a następnie kliknij przycisk **ponowne uruchomienie**.
   
    ![Uruchom ponownie Livy intepreter](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "ponowne uruchomienie Zeppelin intepreter")
1. Uruchomić komórkę kodu z istniejących Notes Zeppelin. Spowoduje to utworzenie nowej sesji usługi Livy w klastrze HDInsight.

## <a name="seealso"></a>Zobacz też
* [Przegląd: platforma Apache Spark w usłudze Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze
* [Platforma Spark i analiza biznesowa: interakcyjna analiza danych na platformie Spark w usłudze HDInsight z użyciem narzędzi do analizy biznesowej](apache-spark-use-bi-tools.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do analizy temperatury w budynku z użyciem danych HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do przewidywania wyników kontroli żywności](apache-spark-machine-learning-mllib-ipython.md)
* [Analiza dzienników witryny sieci Web na platformie Spark w usłudze HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Tworzenie i uruchamianie aplikacji
* [Tworzenie autonomicznych aplikacji przy użyciu języka Scala](apache-spark-create-standalone-application.md)
* [Zdalne uruchamianie zadań w klastrze Spark przy użyciu programu Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia
* [Tworzenie i przesyłanie aplikacji Spark Scala przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](apache-spark-intellij-tool-plugin.md)
* [Zdalne debugowanie aplikacji Spark przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami
* [Zarządzanie zasobami klastra Apache Spark w usłudze Azure HDInsight](apache-spark-resource-manager.md)
* [Śledzenie i debugowanie zadań uruchamianych w klastrze Apache Spark w usłudze HDInsight](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md 







