@Configuration
@Slf4j
@Order(1)
public class InitDataCountryConfig {

    private final CountryRepository countryRepository;
    @Value("classpath:namematching/legalforms/be.json")
    private Resource beDictResource;
    @Value("classpath:namematching/legalforms/eu.json")
    private Resource euDictResource;
    @Value("classpath:namematching/legalforms/no.json")
    private Resource noDictResource;
    @Value("classpath:namematching/legalforms/pl.json")
    private Resource plDictResource;
    @Value("classpath:namematching/legalforms/uk.json")
    private Resource ukDictResource;
    @Value("classpath:namematching/legalforms/us.json")
    private Resource usDictResource;
    @Value("classpath:namematching/legalforms/fr.json")
    private Resource frDictResource;
    @Value("classpath:namematching/legalforms/de.json")
    private Resource deDictResource;
    @Value("classpath:namematching/legalforms/nl.json")
    private Resource nlDictResource;
    @Value("classpath:namematching/legalforms/se.json")
    private Resource seDictResource;

    @Value("classpath:namematching/countries/countries.json")
    private Resource countryListResource;

    @Autowired
    private ObjectMapper objectMapper;

    public InitDataCountryConfig(CountryRepository countryRepository) {
        this.countryRepository = countryRepository;
    }

    @PostConstruct
    public void initDb() throws IOException {
        /*log.info("Loading data country to populate database");
        Set<DictionaryEntity> countryEntityIteratorLegalForm = List.of(beDictResource, euDictResource, noDictResource, plDictResource, ukDictResource, usDictResource, frDictResource, deDictResource, nlDictResource, seDictResource).stream().map(resource -> {
            try {
                DictionaryEntity countryLegalForm = getCountryEntityFromResource(resource);
                countryLegalForm.setType(Type.LEGAL_FORM);
                return countryLegalForm;
            } catch (IOException e) {
                log.error("Error loading  resources : {}", e);
                throw new WebServerException("Error loading  resources : {}", e);
            }
        }).collect(Collectors.toSet());

        DictionaryEntity country = getCountryEntityFromResource(countryListResource);
        country.setType(Type.GEOGRAPHY);
        Optional<DictionaryEntity> countryCheck = this.countryRepository.findOne(Example.of(country));
        if (countryCheck.isEmpty()) {
            this.countryRepository.save(country);
        }

        // check if data is present in database
        countryEntityIteratorLegalForm.forEach(countryEntity -> {
            Optional<DictionaryEntity> countryLegalForm = this.countryRepository.findOne(Example.of(countryEntity));
            if (countryLegalForm.isEmpty()) {
                this.countryRepository.save(countryEntity);
            }
        });*/
    }

    private DictionaryEntity loadDataInDatabase(Resource dictionaryResource) throws IOException {
        Reader dictionaryReader = Files.newBufferedReader(Path.of(dictionaryResource.getURI().getPath()));
        return this.objectMapper.readValue(dictionaryReader, DictionaryEntity.class);
    }

    private DictionaryEntity getCountryEntityFromResource(Resource seDictResource) throws IOException {
        DictionaryEntity country = loadDataInDatabase(seDictResource);
        country.getForms().stream().forEach(formEntity -> formEntity.storeCountry(country));
        return country;
    }
}
```
```
