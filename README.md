{ pkgs, ... }: {
  # Qual canal do nixpkgs usar.
  canal = "stable-24.05"; # ou "unstable"
  # Use https://search.nixos.org/packages para encontrar pacotes
  pacotes = [
    pkgs.jdk17
    pkgs.unzip
  ];
  # Define variáveis de ambiente no workspace
  env = {};
  idx = {
    # Procure as extensões desejadas em https://open-vsx.org/ e use "publisher.id"
    extensões = [
      "Dart-Code.flutter"
      "Dart-Code.dart-code"
    ];
    workspace = {
      # Executa quando um workspace é criado pela primeira vez com este arquivo `dev.nix`
      onCreate = {
        build-flutter = ''
          cd /home/user/myapp/android

          ./gradlew \
            --parallel \
            -Pverbose=true \
            -Ptarget-platform=android-x86 \
            -Ptarget=/home/user/myapp/lib/main.dart \
            -Pbase-application-name=android.app.Application \
            -Pdart-defines=RkxVVFRFUl9XRUJfQ0FUVFNFUklfVVJMPWh0dHBzOi8vd3d3LmdzdGF0aWMuY29tL2ZsdXR0ZXItY2FudmFza2l0Lzk3NTUwOTA3YjcwZjRmM2IzMjhiNmMxNjAwZGYyMWZhYzFhMTg4OWEv \
            -Pdart-obfuscation=false \
            -Ptrack-widget-creation=true \
            -Ptree-shake-icons=false \
            -Pfilesystem-scheme=org-dartlang-root \
            assembleDebug

          # TODO: Execute a construção web no modo de depuração.
          # flutter run faz isso de forma transparente
          # https://github.com/flutter/flutter/issues/96283#issuecomment-1144750411
          # flutter build web --profile --dart-define=Dart2jsOptimization=O0
        '';
      };
      
      # Para executar algo cada vez que o workspace é (re)iniciado, use o hook `onStart`
    };
    # Habilitar previews e personalizar configuração
    previews = {
      enable = true;
      previews = {
        web = {
          command = ["flutter" "run" "--machine" "-d" "web-server" "--web-hostname" "0.0.0.0" "--web-port" "$PORT"];
          manager = "flutter";
        };
        android = {
          command = ["flutter" "run" "--machine" "-d" "android" "-d" "localhost:5555"];
          manager = "flutter";
        };
      };
    };
  };
}
