@Component
public class DataLoader implements CommandLineRunner {

    @Autowired
    private OdontologoRepository odontologoRepository;

    @Autowired
    private PacienteRepository pacienteRepository;

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private RoleRepository roleRepository;

    @Autowired
    private TurnoRepository turnoRepository;

    @Override
    public void run(String... args) {
        // Agregar roles
        Role roleAdmin = new Role();
        roleAdmin.setName("ROLE_ADMIN");
        roleRepository.save(roleAdmin);

        Role roleUser = new Role();
        roleUser.setName("ROLE_USER");
        roleRepository.save(roleUser);

        // Agregar usuarios
        User adminUser = new User();
        adminUser.setUsername("admin");
        adminUser.setPassword(passwordEncoder().encode("admin"));
        adminUser.setEnabled(true);
        adminUser.setRoles(new HashSet<>(Collections.singletonList(roleAdmin)));
        userRepository.save(adminUser);

        User normalUser = new User();
        normalUser.setUsername("usuario");
        normalUser.setPassword(passwordEncoder().encode("password"));
        normalUser.setEnabled(true);
        normalUser.setRoles(new HashSet<>(Collections.singletonList(roleUser)));
        userRepository.save(normalUser);

        // Agregar odontólogos
        Odontologo odontologo1 = new Odontologo();
        odontologo1.setNombre("Carlos");
        odontologo1.setApellido("González");
        odontologo1.setMatricula("OG12345");
        odontologoRepository.save(odontologo1);

        Odontologo odontologo2 = new Odontologo();
        odontologo2.setNombre("Ana");
        odontologo2.setApellido("Martínez");
        odontologo2.setMatricula("AM67890");
        odontologoRepository.save(odontologo2);

        // Agregar pacientes
        Paciente paciente1 = new Paciente();
        paciente1.setNombre("Juan");
        paciente1.setApellido("Pérez");
        paciente1.setDomicilio("Calle 123, Ciudad");
        paciente1.setDni("12345678");
        paciente1.setFechaAlta(LocalDate.now());
        pacienteRepository.save(paciente1);

        Paciente paciente2 = new Paciente();
        paciente2.setNombre("María");
        paciente2.setApellido("Gómez");
        paciente2.setDomicilio("Avenida XYZ, Pueblo");
        paciente2.setDni("87654321");
        paciente2.setFechaAlta(LocalDate.now());
        pacienteRepository.save(paciente2);

        // Agregar turnos (asignar algunos turnos a pacientes y odontólogos)
        Turno turno1 = new Turno();
        turno1.setPaciente(paciente1);
        turno1.setOdontologo(odontologo1);
        turno1.setFechaHora(LocalDateTime.now().plusDays(1));
        turnoRepository.save(turno1);

        Turno turno2 = new Turno();
        turno2.setPaciente(paciente2);
        turno2.setOdontologo(odontologo2);
        turno2.setFechaHora(LocalDateTime.now().plusDays(2));
        turnoRepository.save(turno2);
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
