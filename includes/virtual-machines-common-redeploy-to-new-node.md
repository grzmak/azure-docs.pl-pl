## <a name="use-the-azure-portal"></a>Korzystanie z witryny Azure Portal
1. Wybierz maszynę Wirtualną, o których chcesz ponownie wdrożyć, a następnie wybierz *ponownie wdrożyć* znajdujący się w *ustawienia* bloku. Konieczne może być przewiń w dół, zobacz **pomoc techniczna i rozwiązywanie problemów** sekcja, która zawiera przycisk "Wdróż ponownie", jak w poniższym przykładzie:
   
    ![Bloku maszyny Wirtualnej platformy Azure](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Aby potwierdzić operację, wybierz pozycję *ponownie wdrożyć* przycisku:
   
    ![Ponowne wdrażanie bloku maszyny Wirtualnej](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. **Stan** maszyny wirtualnej zmieni się na *aktualizowanie* jako maszyna wirtualna przygotowuje się do ponownego wdrożenia, jak pokazano w poniższym przykładzie:
   
    ![Aktualizowanie maszyny Wirtualnej](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. **Stan** zmienia się na *od* jako maszyna wirtualna zostanie uruchomiona na nowego hosta platformy Azure, jak pokazano w poniższym przykładzie:
   
    ![Uruchamianie maszyny wirtualnej](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Po maszyny Wirtualnej kończy proces rozruchu **stan** powraca do *systemem*, wskazującą, maszyna wirtualna ma zostały pomyślnie wdrożono ponownie:
   
    ![Działająca maszyna wirtualna](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

