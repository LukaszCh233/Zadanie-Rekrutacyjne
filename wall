
//Zadanie nie jest dokadnie opisane i ciężko domyślić się czego poństwo dokładnie oczekują więc zrobiłem to w taki sposób
//Będąc szczerym zrobienie tego w 100% samemu było w moim przypadku niemożliwe, więc skorzystałem z internetu
//Wspomagałem się różnymi źródłami aby móc bardziej zrozumieć dokładnie zadanie w którym compositeBlock sprawiło problem
// Jestem bardzo ciekawy jakie jest dokładne rozwiązanie tego zadania i czego państwo oczekują

public class Wall implements Structure {
    private List<Block> blocks;

    public Wall(List<Block> blocks) {
        this.blocks = blocks;
    }

    //Zwracany jest cały compositeBlock jeśli znajduje się w nim pasujący blok, nie wiem czy o to chodziło
    @Override
    public Optional<Block> findBlockByColor(String color) {
        Optional<Block> result = blocks.stream()
                .filter(block -> isColor(block, color))
                .findFirst();
        if (result.isEmpty()) {
            result = blocks.stream()
                    .filter(this::isComposite)
                    .map(block -> findBlockByColorInComposite(color, (CompositeBlock) block))
                    .flatMap(Optional::stream)
                    .findFirst();
        }
        return result;
    }

    //Nie do końca rozumiem czego państwo oczekują, jak konkretnie wygląda compositeBlock
    //Założyłem, że ma on własny kolor i materiał a dodatkowo może składać się z innych bloków
    @Override
    public List<Block> findBlocksByMaterial(String material) {
        return blocks.stream()
                .map(block -> findBlocksByMaterialInBlocksAndInCompositeBlock(material, block))
                .flatMap(Collection::stream)
                .collect(Collectors.toList());
    }

    //Najpierw przeszukiwany jest compositeBlock i jego elementy
    //Dalsza część sprawdza czy w compositeBlock znajduje się inny compositeBlock jeśli tak to go przeszukuje
    private Optional<Block> findBlockByColorInComposite(String color, CompositeBlock compositeBlock) {
        for (Block block : compositeBlock.getBlocks()) {
            if (isColor(block, color)) {
                return Optional.of(block);
            }
            if (isComposite(block)) {
                Optional<Block> result = findBlockByColorInComposite(color, (CompositeBlock) block);
                if (result.isPresent()) {
                    return result;
                }
            }
        }
        return Optional.empty();
    }

    //Sprawdzane są pojedzyncze bloki, następnie sprawdzanie czy jest jakiś compositeBlock jeśli tak to go sprawdza
    private List<Block> findBlocksByMaterialInBlocksAndInCompositeBlock(String material, Block block) {
        List<Block> result = new ArrayList<>();

        if (isMaterial(block, material)) {
            result.add(block);
        }
        if (isComposite(block)) {
            for (Block innerBlock : ((CompositeBlock) block).getBlocks()) {
                result.addAll(findBlocksByMaterialInBlocksAndInCompositeBlock(material, innerBlock));
            }
        }
        return result;
    }

    @Override
    public int count() {
        return countBlocks(blocks);
    }

    private boolean isColor(Block block, String color) {
        return block.getColor().equalsIgnoreCase(color);
    }

    private boolean isComposite(Block block) {
        return block instanceof CompositeBlock;
    }

    private boolean isMaterial(Block block, String material) {
        return block.getMaterial().equalsIgnoreCase(material);
    }

/*Zlicza liczbę bloków a następnie sprawdza czy jest jakiś compositeBlock, jeśli tak to do count dodawana jest liczba
bloków znajdujących sie w compositeBlock
 */

    private int countBlocks(List<Block> blocks) {
        int count = 0;

        for (Block block : blocks) {
            count++;
            if (block instanceof CompositeBlock) {
                count += countBlocks(((CompositeBlock) block).getBlocks());
            }
        }
        return count;
    }
}
